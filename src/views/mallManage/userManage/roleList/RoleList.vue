<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>用户管理</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>角色列表</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>角色列表</span>
            <br />
            <span style='float: right'>
                <el-button  size='1'>更新</el-button>
                <el-button @click='addRoleDrawer = true' type='primary' size='1'>新增</el-button>
            </span>
        </el-header>
        <el-main>
            <el-row>
                <el-col class='col_top' :span="2">序号</el-col>
                <el-col class='col_top' :span="3">角色名称</el-col>
                <el-col class='col_top' :span="4">描述</el-col>
                <el-col class='col_top' :span="2">用户数</el-col>
                <el-col class='col_top' :span="4">添加时间</el-col>
                <el-col class='col_top' :span="3">是否启用</el-col>
                <el-col class='col_top' :span="6">操作</el-col>
            </el-row>
            <el-row v-for="(item,index) of userList" :key='index'>
                <el-col :span="2">{{item.id}}</el-col>
                <el-col :span="3">{{item.roleName}}</el-col>
                <el-col :span="4">{{item.description}}</el-col>
                <el-col :span="2">{{item.quantity}}</el-col>
                <el-col :span="4">{{item.addDate}}</el-col>
                <el-col :span="3"><el-switch v-model="item.status"></el-switch></el-col>
                <el-col :span="6" >
                    <span><el-link @click='editRoleDrawer = true' :underline="false" style='color: #409EFF'>编辑</el-link></span>
                    <span><el-link @click='delRoleDialog = true' :underline="false" style='color: #409EFF'>删除</el-link></span>
                    <span><router-link to="/distributeMenu" :underline="false" style='color: #409EFF'>分配菜单</router-link></span>
                    <span><router-link to="/distributeResource" :underline="false" style='color: #409EFF'>分配资源</router-link></span>
                </el-col>
            </el-row>
        </el-main>
        <!--删除确认-->
        <el-dialog :visible.sync='delRoleDialog' top='250px'>
            <div slot="title" class="dialog-title">
                <i class="el-icon-warning"  style="font-size:18px;color:rgb(250,173,20); "></i>&nbsp;&nbsp;
                <span class="title-text"><b>删除角色确认</b></span>
                <div class="button-right">
                    <span class="title-close" @click="delRoleDialog = false "></span>
                </div>
            </div>
            <div>
                <p>您确定要删除该角色吗？</p>
                <br />
                <el-button @click='delRoleDialog = false'>取消</el-button>
                <el-button type='primary' @click='delRole()'  plain>确定</el-button>
            </div>
        </el-dialog>

        <!--打开用户的添加-->
        <el-drawer
            size="20%"
            title="新增角色"
            :visible.sync="addRoleDrawer"
            :direction="rtl"
        >
            <div>
                <el-row><b>角色名称</b></el-row>
                <el-row>
                    <el-input placeholder='请输入角色名称' v-model="nameText"  maxlength="10" show-word-limit></el-input>
                </el-row>
                <el-row><b>描述</b></el-row>
                <el-row>
                    <el-input v-model="descriptionText" placeholder='请简要描述角色功能权限'></el-input>
                </el-row>
            </div>
            <el-divider></el-divider>
            <div style='text-align: center'>
                <el-button type='primary' @click='saveAddRole()'>保存</el-button>
                <el-button @click='addRoleDrawer = false'>取消</el-button>
            </div>
        </el-drawer>

        <!--打开用户的编辑-->
        <el-drawer
            size="20%"
            title="编辑用户的信息"
            :visible.sync="editRoleDrawer"
            :direction="rtl"
        >
            <div>
                <el-row><b>角色名称</b></el-row>
                <el-row>
                    <el-input v-model="roleNameText" maxlength="10" show-word-limit></el-input>
                </el-row>
                <el-row><b>描述</b></el-row>
                <el-row>
                    <el-input v-model="roleDescriptionText"></el-input>
                </el-row>
            </div>
            <el-divider></el-divider>
            <div style='text-align: center'>
                <el-button @click='saveEditRole()' type='primary'>保存</el-button>
                <el-button @click='editRoleDrawer = false'>取消</el-button>
            </div>
        </el-drawer>
    </el-container>
</template>

<script>
export default {
    name: 'RoleList',
    data(){
        return{
            editRoleDrawer: false,  //用户修改页面抽屉显示
            addRoleDrawer: false,   //角色添加页面抽屉显示
            delRoleDialog:false,    //删除用户确定弹窗
            addRoleDialog:false,    //添加用户确定弹窗
            editRoleDialog:false,    //修改用户确定弹窗
            nameText:"",        //添加角色名
            descriptionText:"",     //添加角色描述
            roleNameText:"技术客服",        //编辑角色名
            roleDescriptionText:"用户解决订单中的技术问题",     //编辑角色描述
            userList:[      //角色列表的信息
                {
                    id:1,
                    roleName:"技术客服",
                    description:"处理订单的技术问题",
                    quantity:12,
                    addDate:"2011/11/13 13:10:02",
                    status:0,   //禁用状态
                },
                {
                    id:1,
                    roleName:"商务客服",
                    description:"处理订单的报价问题",
                    quantity:3,
                    addDate:"2011/11/13 13:10:02",
                    status:true,   //禁用状态
                }
            ],
        }
    },
    methods:{
        //添加新角色
        saveAddRole(){
            this.addRoleDrawer = false;
        },
        //修改角色信息
        saveEditRole(){
            alert("修改成功");
            this.editRoleDrawer = false;
        },
        //删除角色信息
        delRole(){
            this.$notify({
                message: '删除成功',
                type: 'success'
            });
            this.delRoleDialog = false;
        }
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
.el-row .el-col span{
    margin-right: 20px;
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
    width: 20%;
    height:150px;
}
.el-dialog__wrapper >>> .el-dialog__body{
    padding: 10px;
    text-align: center;
}
</style>