<template>
  <el-container>
    <el-header height='120px'>
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>用户管理</el-breadcrumb-item>
        <el-breadcrumb-item><a href='/'>用户列表</a></el-breadcrumb-item>
      </el-breadcrumb>
      <br />
      <span style='font-size: 18px;font-weight: bold'>用户列表</span>
      <el-row>
        <el-form :inline="true" :model="listQuery" size="small">
        <el-col :span="8">
          <div style="width: 100%">
            <span style='float: right'>&nbsp;</span>
          </div>
        </el-col>
        <el-col :span="8">
          <el-input size='1' v-model="listQuery.keyword" class="input-width" placeholder="请输入关键词搜索" clearable></el-input>
        </el-col>
        <el-col :span="8">
          <el-button
              style="float:left"
              type="primary"
              @click="handleSearchList()"
              size="1">
            搜索
          </el-button>
        </el-col>
        </el-form>
      </el-row>
    </el-header>
    <el-main>
      <el-card class="operate-container" shadow="never">
        <span size="1">列表显示&nbsp;&nbsp;</span>
        <el-select size='1' v-model="roleValue" filterable placeholder="请选择">
          <el-option
              v-for="item in roleType"
              :key="item.value"
              :label="item.label"
              :value="item.value">
          </el-option>
        </el-select>
        <span style='float: right'>
          <el-button @click='addUserDrawer = true' type='primary' size='1' style='text-align: right'>添加用户</el-button>
        </span>
      </el-card>

      <el-divider></el-divider>
      <!--    用户列表信息    -->

      <div class="table-container">
        <el-table ref="adminTable"
                  :data="list"
                  style="width: 100%;"
                  v-loading="listLoading" border>
          <el-table-column label="序号" width="100" align="center">
            <template slot-scope="scope">{{scope.row.id}}</template>
          </el-table-column>
          <el-table-column label="账号" align="center">
            <template slot-scope="scope">{{scope.row.username}}</template>
          </el-table-column>
          <el-table-column label="姓名" align="center">
            <template slot-scope="scope">{{scope.row.nickName}}</template>
          </el-table-column>
          <el-table-column label="角色" align="center">
            <template slot-scope="scope">{{scope.row.nickName}}</template>
          </el-table-column>
          <el-table-column label="添加时间" width="160" align="center">
            <template slot-scope="scope">{{scope.row.createTime | formatDateTime}}</template>
          </el-table-column>
          <el-table-column label="最近登录" width="160" align="center">
            <template slot-scope="scope">{{scope.row.loginTime | formatDateTime}}</template>
          </el-table-column>
          <el-table-column label="是否启用" width="140" align="center">
            <template slot-scope="scope">
              <el-switch
                  @change="handleStatusChange(scope.$index, scope.row)"
                  :active-value="1"
                  :inactive-value="0"
                  v-model="scope.row.status">
              </el-switch>
            </template>
          </el-table-column>
          <el-table-column label="操作" width="180" align="center">
            <template slot-scope="scope">
              <el-button size="mini"
                         type="text"
                         @click="handleUpdate(scope.$index, scope.row)">
                编辑
              </el-button>
              <el-button size="mini"
                         type="text"
                         @click="handleDelete(scope.$index, scope.row)">删除
              </el-button>
              <el-button size="mini"
                         type="text"
                         @click="handleSelectRole(scope.$index, scope.row)">分配角色
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div class="pagination-container">
        <el-pagination
            background
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            layout="total, sizes,prev, pager, next,jumper"
            :current-page.sync="listQuery.pageNum"
            :page-size="listQuery.pageSize"
            :page-sizes="[10,15,20]"
            :total="total">
        </el-pagination>
      </div>
    </el-main>
    <!--删除确认-->
    <el-dialog :visible.sync='delUserDialog' center>
      <div slot="title" class="dialog-title">
        <i class="el-icon-warning"  style="font-size:18px;color:rgb(250,173,20); "></i>&nbsp;&nbsp;
        <span class="title-text"><b>删除用户确认</b></span>
        <div class="button-right">
          <span class="title-close" @click="delUserDialog = false "></span>
        </div>
      </div>
      <div>
        <p>您确定要删除当前用户吗？</p>
        <br />
        <el-button @click='delUserDialog = false'>取消</el-button>
        <el-button type='primary' @click='delUserDialog = false'>确定</el-button>
      </div>
    </el-dialog>

    <!--打开用户的添加-->
    <el-drawer
        size="20%"
        title="添加用户的信息"
        :visible.sync="addUserDrawer"
        direction="rtl"
    >
      <div>
        <el-row><b>账号</b></el-row>
        <el-row>
          <el-input placeholder='请输入用户账号'></el-input>
        </el-row>
        <el-row><b>姓名</b></el-row>
        <el-row>
          <el-input placeholder='请输入用户姓名'></el-input>
        </el-row>
        <el-row><b>分配角色</b></el-row>
        <el-row>
          <el-select size='1' v-model="addRoleValue" filterable placeholder="请选择角色类型">
            <el-option
                v-for="item in roleType2"
                :key="item.value"
                :label="item.label"
                :value="item.value">
            </el-option>
          </el-select>
        </el-row>
        <el-row><b>密码</b></el-row>
        <el-row>
          <el-input placeholder='请设置用户密码'></el-input>
        </el-row>
      </div>
      <el-divider></el-divider>
      <div style='text-align: center'>
        <el-button type='primary' @click='addUserDialog = true'>保存</el-button>
        <el-button @click='addUserDrawer = false'>取消</el-button>
      </div>
      <el-dialog class='dialog11'
                 :visible.sync='addUserDialog'
                 append-to-body>
        <div slot="title" class="dialog-title">
          <i class="el-icon-warning"  style="font-size:18px;color:rgb(250,173,20); "></i>&nbsp;&nbsp;
          <span class="title-text"><b>保存信息确认</b></span>
          <div class="button-right">
            <span class="title-close" @click="addUserDialog = false "></span>
          </div>
        </div>
        <div>
          <p>您确定要保存当前信息吗？</p>
          <br />
          <el-button type='primary' @click='addUserDialog = false'>确定</el-button>
          <el-button @click='addUserDialog = false'>取消</el-button>
        </div>
      </el-dialog>
    </el-drawer>

    <!--打开用户的编辑-->
    <el-drawer
        size="20%"
        title="编辑用户的信息"
        :visible.sync="editUserDrawer"
        direction="rtl"
    >
      <div>
        <el-row><b>账号</b></el-row>
        <el-row>
          <el-input value='124455'></el-input>
        </el-row>
        <el-row><b>姓名</b></el-row>
        <el-row>
          <el-input value='ssss'></el-input>
        </el-row>
        <el-row><b>分配角色</b></el-row>
        <el-row>
          <el-select size='1' v-model="editRoleValue" filterable placeholder="请选择">
            <el-option
                v-for="item in roleType2"
                :key="item.value"
                :label="item.label"
                :value="item.value">

            </el-option>
          </el-select>
        </el-row>
        <el-row><b>密码</b></el-row>
        <el-row>
          <el-input value='11111'></el-input>
        </el-row>
      </div>
      <el-divider></el-divider>
      <div style='text-align: center'>
        <el-button @click='editUserDialog = true' type='primary'>保存</el-button>
        <el-button @click='editUserDrawer = false'>取消</el-button>
      </div>
      <el-dialog  class='dialog11'
                  :visible.sync='editUserDialog'
                  append-to-body>
        <div slot="title" class="dialog-title">
          <i class="el-icon-warning"  style="font-size:18px;color:rgb(250,173,20); "></i>&nbsp;&nbsp;
          <span class="title-text"><b>用户信息修改</b></span>
          <div class="button-right">
            <span class="title-close" @click="editUserDialog = false "></span>
          </div>
        </div>
        <div>
          <p>您确定要保存修改信息吗？</p>
          <br />
          <el-button type='primary' @click='editUserDialog = false'>确定</el-button>
          <el-button @click='editUserDialog = false'>取消</el-button>
        </div>
      </el-dialog>
    </el-drawer>

  </el-container>
</template>

<script>
import {fetchList,getRoleByAdmin,updateStatus,deleteAdmin} from '@/api/login';
import {formatDate} from '@/utils/date';

const defaultListQuery = {
  pageNum: 1,
  pageSize: 10,
  keyword: null
};
export default {
  name: 'UserList',
  data(){
    return{
      listQuery: Object.assign({}, defaultListQuery),
      editUserDrawer: false,  //用户修改页面抽屉显示
      addUserDrawer: false,   //用户添加页面抽屉显示
      roleValue: "全部",     //按该类型展示用户列表
      addRoleValue:"",    //添加用户类型的默认值
      editRoleValue:"技术客服",   //修改用户类型的默认值
      delUserDialog:false,    //删除用户确定弹窗
      addUserDialog:false,    //添加用户确定弹窗
      editUserDialog:false,    //修改用户确定弹窗
      list: null,
      total: null,
      listLoading: false,
      userList:[      //用户列表的信息
        {
          id:1,
          phone:1234545,
          username:"ssss",
          userType:"商务客服",
          addDate:"2011/11/13",
          percentDate:"2011/12/13",
          status:0,   //禁用状态
        },
        {
          id:2,
          phone:1234545,
          username:"ssss",
          userType:"技术客服",
          addDate:"2011/11/13",
          percentDate:"2011/12/13",
          status:true,    //禁用状态
        }
      ],
      roleType: [{        //用户列表展示选择角色类型
        value: '*',
        label: '全部'
      }, {
        value: '技术客服',
        label: '技术客服'
      }, {
        value: '商务客服',
        label: '商务客服'
      }, {
        value: '管理员',
        label: '管理员'
      }],
      roleType2: [{       //用户添加/修改可选角色类型
        value: '技术客服',
        label: '技术客服'
      }, {
        value: '商务客服',
        label: '商务客服'
      }, {
        value: '管理员',
        label: '管理员'
      }],
    }
  },
  filters: {
    formatDateTime(time) {
      if (time == null || time === '') {
        return 'N/A';
      }
      let date = new Date(time);
      return formatDate(date, 'yyyy-MM-dd hh:mm:ss')
    }
  },
  methods:{
    test(){
      alert(111);
    },
    editUser(){     //编辑所选用户信息

    },
    handleSearchList() {
      this.listQuery.pageNum = 1;
      this.getList();
    },
    handleSelectRole(index,row){
      this.allocAdminId = row.id;
      this.allocDialogVisible = true;
      this.getRoleListByAdmin(row.id);
    },
    getRoleListByAdmin(adminId) {
      getRoleByAdmin(adminId).then(response => {
        let allocRoleList = response.data;
        this.allocRoleIds=[];
        if(allocRoleList!=null&&allocRoleList.length>0){
          for(let i=0;i<allocRoleList.length;i++){
            this.allocRoleIds.push(allocRoleList[i].id);
          }
        }
      });
    },
    handleStatusChange(index, row) {
      this.$confirm('是否要修改该状态?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        updateStatus(row.id, {status: row.status}).then(response => {
          this.$message({
            type: 'success',
            message: '修改成功!'
          });
        });
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '取消修改'
        });
        this.getList();
      });
    },
    handleDelete(index, row) {
      this.$confirm('是否要删除该用户?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        deleteAdmin(row.id).then(response => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          this.getList();
        });
      });
    },
    handleUpdate(index, row) {
      this.dialogVisible = true;
      this.isEdit = true;
      this.admin = Object.assign({},row);
    },
    handleSizeChange(val) {
      this.listQuery.pageNum = 1;
      this.listQuery.pageSize = val;
      this.getList();
    },
    handleCurrentChange(val) {
      this.listQuery.pageNum = val;
      this.getList();
    },
    getList() {
      this.listLoading = true;
      fetchList(this.listQuery).then(response => {
        this.listLoading = false;
        this.list = response.data.list;
        this.total = response.data.total;
      });
    },
  }
};
</script>

<style scoped>
.content{
  padding: 0;
  font-size: 16px;
}
.el-header {
  background-color: white;
}
.el-breadcrumb{
  margin-top: 10px;
}
.el-main{
  margin: 15px;
  background-color: white;
  font-size: 14px;
}
.el-main .el-col{
  height: 50px;
  line-height: 50px;
  border-bottom: 1px solid rgb(240,240,240);
}
.col_top{
  font-weight: bold;
  background-color: rgb(245,245,245);
}
/*  抽屉自定义设置 */
/deep/ .el-drawer__header{
  color: black;
  font-weight: bold;
  padding-bottom: 20px;
  margin-bottom: 10px;
  border-bottom:1px solid rgb(240,240,240);
}
/deep/ .el-drawer__body{
  padding: 5px 15px;
}
.el-drawer .el-row{
  margin-bottom: 10px;
}
.el-dialog__wrapper  >>> .el-dialog{
  float: right;
  width: 20%;
  height:150px;
}
.el-dialog__wrapper >>> .el-dialog__body{
  padding: 10px;
  text-align: center;
}
</style>
