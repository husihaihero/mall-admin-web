<template>
    <el-container>
        <el-header height='30%'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>质检管理</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>质检管理</span>
            <!--    顶部搜索框      -->
            <el-row style='width:100%;text-align: center;padding:5px'>
                <el-input style='width: 500px;'
                          size='1'
                          placeholder='请输入关键字搜索'
                ></el-input>
                <el-button type='primary' size='1'>搜索</el-button>
            </el-row>
            <div class="customerType">
                <el-row>
                    <a><el-col :span='6'>全部</el-col></a>
                    <a><el-col :span='6'>已申请</el-col></a>
                    <a><el-col :span='6'>未申请</el-col></a>
                    <a><el-col :span='6'>已开具</el-col></a>
                </el-row>
            </div>
        </el-header>
        <el-main>
            <div>
                <el-row>
                    <el-col class='col_top col_first' :span="2">序号</el-col>
                    <el-col class='col_top' :span="4">客户名称</el-col>
                    <el-col class='col_top' :span="3">订单详情</el-col>
                    <el-col class='col_top' :span="3">质检状态</el-col>
                    <el-col class='col_top' :span="3">质检方式</el-col>
                    <el-col class='col_top' :span="4">订单类型</el-col>
                    <el-col class='col_top' :span="5">操作</el-col>
                </el-row>
                <el-row v-for="(item,index) of allOrderList" :key='index'>
                    <el-col :span="2"  class='col_first'>{{item.id}}</el-col>
                    <el-col :span="4">{{item.username}}</el-col>
                    <el-col :span="3"><router-link class='clickStyle' to="/orderTestingInfo">订单详情</router-link>&nbsp;</el-col>
                    <el-col :span="3">{{item.testingStatus}}</el-col>
                    <el-col :span="3">{{item.testingType}}</el-col>
                    <el-col :span="4">{{item.orderType}}</el-col>
                    <el-col  :span="5">
                        <span class='clickStyle'>上传报告</span>&nbsp;&nbsp;
                    </el-col>
                </el-row>
            </div>
            <br />
            <!--            分页      -->
            <el-pagination
                background
                style='float: right;'
                layout="prev, pager, next"
                :total="1000">
            </el-pagination>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'qualityTestingList',
    data() {
        return {
            payType:'1',
            activeName: 'first',
            allOrderList:[       //所有订单
                {
                    id:1,
                    username:"张三",
                    testingStatus:"已申请",
                    testingType:"申亿质检",
                    orderType:"非标件",
                    operation:'上传报告',
                },
                {
                    id: 2,
                    username: "李四",
                    testingStatus: "已申请",
                    testingType: "第三方质检",
                    orderType: "非标件",
                    operation: '上传报告'
                },
                {
                    id:3,
                    username:"有限公司",
                    testingStatus:"已申请",
                    testingType:"申亿质检",
                    orderType:"非标件",
                    operation:'上传报告',
                }
            ],

        };
    },
    methods: {
        handleClick(tab, event) {
            console.log(tab, event);
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
.customerType{
    font-size: 15px;
    width: 40%;
    margin:0 auto;
    text-align: center;
}
.customerType .el-col{
    cursor: pointer;    /*放上去边手型*/
}

.customerType .el-col:hover{
    color: #2d8cf0;
    border-bottom: 2px solid #2d8cf0;
}
.el-col{
    height: 50px;
    line-height: 50px;
    border-bottom: 1px solid rgb(240,240,240);
}
.col_top{
    font-weight: bold;
    background-color: rgb(245,245,245);
}
.col_first{
    padding-left: 20px;
}
</style>
