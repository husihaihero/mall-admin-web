<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>用户管理</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>资源列表</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>资源列表</span>
            <br />
            <span style='float: right'>
                <el-button  size='1'>更新</el-button>
                <el-button @click='addResourceDrawer = true' type='primary' size='1'>新增</el-button>
            </span>
        </el-header>
        <el-main>
            <el-row>
                <el-col class='col_top' :span="2"></el-col>
                <el-col class='col_top' :span="3">资源名称</el-col>
                <el-col class='col_top' :span="4">资源内容</el-col>
                <el-col class='col_top' :span="7">资源描述</el-col>
                <el-col class='col_top' :span="4">添加时间</el-col>
                <el-col class='col_top' :span="4">操作</el-col>
            </el-row>
            <el-row v-for="(item,index) of resourceList" :key='index'>
                <el-col :span="2">{{item.id}}</el-col>
                <el-col :span="3">{{item.resourceName}}</el-col>
                <el-col :span="4">{{item.content}}</el-col>
                <el-col :span="7">{{item.description}}</el-col>
                <el-col :span="4">{{item.addDate}}</el-col>
                <el-col :span="4" >
                    <span><el-link @click='editResourceDrawer = true' :underline="false" style='color: #409EFF'>编辑</el-link></span>
                    <span><el-link @click='delResourceDialog = true' :underline="false" style='color: #409EFF'>删除</el-link></span>
                </el-col>
            </el-row>
        </el-main>
        <!--删除确认-->
        <el-dialog :visible.sync='delResourceDialog' top='250px'>
            <div slot="title" class="dialog-title">
                <i class="el-icon-warning"  style="font-size:18px;color:rgb(250,173,20); "></i>&nbsp;&nbsp;
                <span class="title-text"><b>删除资源确认</b></span>
                <div class="button-right">
                    <span class="title-close" @click="delResourceDialog = false "></span>
                </div>
            </div>
            <div>
                <p>您确定要删除该资源吗？</p>
                <br />
                <el-button @click='delResourceDialog = false'>取消</el-button>
                <el-button type='primary' @click='delResource()'  plain>确定</el-button>
            </div>
        </el-dialog>

        <!--打开资源的添加-->
        <el-drawer
            size="20%"
            title="新增资源"
            :visible.sync="addResourceDrawer"
            :direction="rtl"
        >
            <div>
                <el-row><b>资源名称</b></el-row>
                <el-row>
                    <el-input placeholder='请填写资源名称' v-model="nameText"  maxlength="10" show-word-limit></el-input>
                </el-row>
                <el-row><b>资源内容</b></el-row>
                <el-row>
                    <el-input v-model="contentText" placeholder='请填写资源内容'></el-input>
                </el-row>
                <el-row><b>资源描述</b></el-row>
                <el-row>
                    <el-input v-model="descriptionText" placeholder='请简要描述资源'></el-input>
                </el-row>
            </div>
            <el-divider></el-divider>
            <div style='text-align: center'>
                <el-button type='primary' @click='saveAddResource()'>保存</el-button>
                <el-button @click='addResourceDrawer = false'>取消</el-button>
            </div>
        </el-drawer>

        <!--打开资源的编辑-->
        <el-drawer
            size="20%"
            title="编辑用户的信息"
            :visible.sync="editResourceDrawer"
            :direction="rtl"
        >
            <div>
                <el-row><b>角色名称</b></el-row>
                <el-row>
                    <el-input v-model="resourceNameText" maxlength="10" show-word-limit></el-input>
                </el-row>
                <el-row><b>资源内容</b></el-row>
                <el-row>
                    <el-input v-model="resourceContentText"></el-input>
                </el-row>
                <el-row><b>资源描述</b></el-row>
                <el-row>
                    <el-input v-model="resourceDescriptionText"></el-input>
                </el-row>
            </div>
            <el-divider></el-divider>
            <div style='text-align: center'>
                <el-button @click='saveEditResource()' type='primary'>保存</el-button>
                <el-button @click='editResourceDrawer = false'>取消</el-button>
            </div>
        </el-drawer>
    </el-container>
</template>

<script>
export default {
    name: 'ResourceList',
    data(){
        return{
            editResourceDrawer: false,  //资源修改页面抽屉显示
            addResourceDrawer: false,   //资源添加页面抽屉显示
            delResourceDialog:false,    //删除资源确定弹窗
            addResourceDialog:false,    //添加资源确定弹窗
            editResourceDialog:false,    //修改资源确定弹窗
            nameText:"",        //添加资源名
            contentText:"",    //添加资源内容
            descriptionText:"",     //添加资源描述‘
            resourceNameText:"访问资源",        //编辑资源名
            resourceContentText:"资源内容资源内容资源内容资源内容",         //编辑资源内容
            resourceDescriptionText:"该资源用户***的权限",     //编辑资源描述
            resourceList:[      //资源列表的信息
                {
                    id:1,
                    resourceName:"访问资源",
                    content:"aaaaaaa",
                    description:"能访问一些特定的信息",
                    addDate:"2011/11/13 13:10:02",
                },
                {
                    id:2,
                    resourceName:"系统资源",
                    content:"bbbbb",
                    description:"修改系统的一些配置",
                    addDate:"2011/11/13 13:10:02",
                }
            ],
        }
    },
    methods:{
        //添加新资源
        saveAddResource(){
            this.$notify({
                message: '添加成功',
                type: 'success'
            });
            this.addResourceDrawer = false;
        },
        //修改资源信息
        saveEditResource(){
            alert("修改成功");
            this.editResourceDrawer = false;
        },
        //删除资源信息
        delResource(){
            this.$notify({
                message: '删除成功',
                type: 'success'
            });
            this.delResourceDialog = false;
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