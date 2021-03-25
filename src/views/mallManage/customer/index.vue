<template>
  <el-container>
    <el-header height='30%'>
      <h2>客户管理</h2>
      <el-row style='width:100%;text-align: center;padding:5px'>
        <el-input style='width: 500px;'
                  size='1'
                  placeholder='请输入关键字搜索'
        >
        </el-input>
        <el-button type='primary' size='1'>搜索</el-button>
      </el-row>
      <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleClick">
        <el-menu-item index="total">全部</el-menu-item>
        <el-menu-item index="audit">待审核</el-menu-item>
        <el-menu-item index="enterprise">企业账号</el-menu-item>
        <el-menu-item index="personal">个人账号</el-menu-item>
      </el-menu>
    </el-header>
    <div class="table-container">
      <el-table ref="orderTable"
                :data="allUserList"
                style="width: 100%;"
                v-loading="listLoading" border>
        <el-table-column label="序号" width="80" align="center">
          <template slot-scope="scope">{{scope.row.id}}</template>
        </el-table-column>
        <el-table-column label="客户名称" width="400" align="center">
          <template slot-scope="scope">{{scope.row.username}}</template>
        </el-table-column>
        <el-table-column label="账号类型" width="120" align="center">
          <template slot-scope="scope">{{scope.row.userType}}</template>
        </el-table-column>
        <el-table-column label="手机号码" width="120" align="center">
          <template slot-scope="scope">{{scope.row.phone}}</template>
        </el-table-column>
        <el-table-column label="法人/代理人" align="center">
          <template slot-scope="scope">{{scope.row.agent}}</template>
        </el-table-column>
        <el-table-column label="操作" width="300" align="left">
          <template slot-scope="scope">
            <a style="color: #2d8cf0"
              size="mini"
              @click="handleViewOrder(scope.$index, scope.row)"
            >查看</a>
            &nbsp;&nbsp;
            <a style="color: #2d8cf0"
              size="mini"
              @click="handleUpdateOrder(scope.$index, scope.row)"
              >修改</a>
            &nbsp;&nbsp;
            <a style="color: #2d8cf0"
              size="mini"
              @click='paySetDialog = true'
              >付款设置</a>
            &nbsp;&nbsp;
            <a style="color: #2d8cf0"
              size="mini"
              @click="handleAuditOrder(scope.$index, scope.row)"
              v-show="scope.row.status===2">前往审核</a>
          </template>
        </el-table-column>
      </el-table>
      <!--        分页      -->
      <el-pagination
        background
        style='float: right;'
        layout="prev, pager, next"
        :total="100">
      </el-pagination>
    </div>
    <!--        设置付款弹出层-->
    <el-dialog title="付款设置" :visible.sync="paySetDialog" class='header-title'   width="370px">
      <p>请选择付款方式：</p>
      <el-checkbox-group v-model='payTypeList' style='margin: 15px 0;text-align: center'>
        <el-checkbox label="按期付款"></el-checkbox>
        <el-checkbox label="线下转账"></el-checkbox>
        <el-checkbox label="微信支付"></el-checkbox>
      </el-checkbox-group>
      <div style='width:100%;text-align:center;'>
        <el-button type="primary">确定</el-button>
        <el-button type="primary" @click='paySetDialog = false'>取消</el-button>
      </div>
    </el-dialog>
  </el-container>
</template>

<script>
export default {
  name: 'customerList',
  data() {
    return {
      multipleSelection: [],
      activeIndex: 'enterprise',
      listLoading: false,
      isShow:true,
      payTypeList:["按期付款"],  //选择支付方式
      paySetDialog:false,
      allUserList:[       //所有用户
        {
          id:1,
          username:"个人客户",
          status:0,
          userType:"个人认证",
          phone:12345546,
          agent:'无',
        },
        {
          id:2,
          username:"待审核客户",
          status:2,
          userType:"待审核",
          phone:1555231,
          agent:'无',
        },
        {
          id:3,
          username:"企业客户",
          status:1,
          userType:"企业认证",
          phone:133331,
          agent:'代理人',
        },
        {
          id:1,
          username:"审核未通过客户",
          status:3,
          userType:"个人认证",
          phone:12345546,
          agent:'无',
        }
      ],
    };
  },
  methods: {
    handleViewOrder(index, row){
      this.$router.push({path:'/mallManage/customer/customerDetail',query:{id:row.id}})
    },
    handleUpdateOrder(index, row){
      this.$router.push({path:'/mallManage/customer/customerDetail',query:{id:row.id}})
    },
    handleAuditOrder(index, row){
      this.$router.push({path:'/mallManage/customer/customerDetail',query:{id:row.id}})
    },
    handleClick(tab, event) {
      console.log(tab, event);
    },
  }

}
</script>
<style scoped>
.customerType .el-col{
  cursor: pointer;    /*放上去边手型*/
}
.customerType .colActive{
  border-bottom: 2px solid #2d8cf0;
  color: #2d8cf0;
}
.el-dialog__wrapper >>> .el-dialog__header{
  border-bottom: 1px solid #e8eaec;
  font-size: 16px;
  font-weight: bold;
}
.el-dialog__wrapper >>> .el-dialog__body{
  padding: 20px;
}
</style>
