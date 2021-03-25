<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>系统设置</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>帮助中心</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>帮助中心</span>
        </el-header>
        <el-main>
            <!--    文章查询栏     -->
            <el-row class='orderSearch'>
                <span style='margin:0 40px;font-size: 16px'>文章搜索</span>
                <el-input aria-placeholder='请输入文章关键字' style='width: 25%'></el-input>
                <el-button size='1' type='primary' style='margin-left: 20px'>查询</el-button>
                <el-button size='1' type='primary' style='float: right;margin: 5px 20px 5px 0;'>新建文章</el-button>
            </el-row>
            <el-row>
                <el-col class='col_top col_first' :span="2"><el-checkbox></el-checkbox></el-col>
                <el-col class='col_top' :span="2">序号</el-col>
                <el-col class='col_top' :span="3">文章标题</el-col>
                <el-col class='col_top' :span="5">发布时间</el-col>
                <el-col class='col_top' :span="2">置顶</el-col>
                <el-col class='col_top' :span="5">是否启用</el-col>
                <el-col class='col_top' :span="5">操作</el-col>
            </el-row>
            <el-row v-for="(item,index) of allArticles" :key='index'>
                <el-col :span="2" class='col_first'><el-checkbox></el-checkbox></el-col>
                <el-col :span="2">{{item.id}}</el-col>
                <el-col :span="3">{{item.title}}</el-col>
                <el-col :span="5">{{item.date}}</el-col>
                <el-col :span="2" style='font-size: 18px;'><i class='el-icon-top' @click='setTop()'></i></el-col>
                <el-col :span="5">
                    <el-switch
                        v-model="item.status"
                        active-color="#13ce66"
                        inactive-color="#ff4949">
                    </el-switch>
                </el-col>
                <el-col :span="5">
                    <router-link to='/editArticle' style='color: #2d8cf0'>编辑</router-link>&nbsp;&nbsp;
                    <el-link :underline='false' style='color: #2d8cf0'>删除</el-link>&nbsp;&nbsp;
                    <router-link to='' style='color: #2d8cf0'>选择分类</router-link>&nbsp;&nbsp;
                    <router-link to='/classifyManage' style='color: #2d8cf0'>分类管理</router-link>&nbsp;&nbsp;
                </el-col>
            </el-row>
            <!--            分页      -->
            <div style='float: right;margin-top: 15px'>
                <el-pagination
                    background
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="1"
                    :page-sizes="[100, 200, 300, 400]"
                    :page-size="100"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="400">
                </el-pagination>
            </div>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'OrdinaryOrder',
    data() {
        return {
            allArticles:[
                {
                    id:1,
                    title:"文章的标题1",
                    date:'2021/02/06 18:33:45',
                    status:true, //禁用状态
                },
                {
                    id:5,
                    title:"文章的标题2",
                    date:'2021/02/06 18:33:45',
                    status:0, //禁用状态
                },
                {
                    id:3,
                    title:"文章的标题3",
                    date:'2021/02/06 18:33:45',
                    status:true, //禁用状态
                },
            ]

        }
    },
    methods:{
        //文件上传的一些相关方法
        handleRemove(file) {
            console.log(file);
        },
        handlePictureCardPreview(file) {
            this.dialogImageUrl = file.url;
            this.dialogVisible = true;
        },
        handleDownload(file) {
            console.log(file);
        },
        //分页
        handleSizeChange(val) {
            console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
        },
        //置顶
        setTop(){
            alert("这条文章置顶");
        }
    }


};
</script>

<style scoped>
.content{
    font-size: 16px;
    padding: 0;
}
.el-header{
    background-color: white;
}
.el-breadcrumb{
    margin-top: 10px;
    font-size: 14px;
}
.el-main{
    margin: 15px;
    padding-top: 10px;
    background-color: white;
    font-size: 14px;
}
.orderSearch{
    width: 100%;
    height: 50px;
    line-height: 50px;
    margin-top: 10px;
    margin-bottom: 10px;
    font-weight: bold;
    border:1px solid rgb(240,240,240);
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
    text-align: center;
}
.modal{
    background-color: white;
    opacity: 0.5;
}
</style>