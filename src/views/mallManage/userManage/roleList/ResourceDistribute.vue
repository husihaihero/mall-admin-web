<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>用户管理</el-breadcrumb-item>
                <el-breadcrumb-item>角色列表</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>分配资源</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>分配资源</span>
            <br />
            <span style='float: right'>
                <el-button  size='1'>更新</el-button>
            </span>
        </el-header>

        <el-main>
            <el-row>
                <el-col class='col_top col_first' :span="2"></el-col>
                <el-col class='col_top' :span="4">资源名称</el-col>
                <el-col class='col_top' :span="6">资源内容</el-col>
                <el-col class='col_top' :span="12">资源描述</el-col>
            </el-row>
            <el-row v-for="(item,index) of resourceList" :key='index'>
                <el-col class='col_first' :span="2"><el-checkbox size='1'></el-checkbox></el-col>
                <el-col :span="4">{{item.resourceName}}</el-col>
                <el-col :span="6">{{item.resourceContent}}</el-col>
                <el-col :span="12">{{item.resourceDescription}}</el-col>
            </el-row>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'DistributeResource',
    data(){
        return{
            resourceList:[
                {
                    resourceName:"客户资源",
                    resourceContent:"暂无",
                    resourceDescription:"可以访问到所有客户的信息"
                },
                {
                    resourceName:"用户资源",
                    resourceContent:"暂无",
                    resourceDescription:"可以访问到所有用户"
                },
            ]
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
.col_top{
    font-weight: bold;
    background-color: rgb(245,245,245);
}
.col_first{
    padding-left:15px;
}

</style>