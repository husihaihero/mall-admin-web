<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>订单管理</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>普通订单</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>普通订单</span>
        </el-header>
        <el-main>
            <div class="orderStatus">
                <template v-for="(item,index) of statusList">
                    <span @click='selectAndChange(index,item)' :class='{active:ind == index}'>{{item}}</span>
                    <el-divider direction='vertical'></el-divider>
                </template>
            </div>
<!--            订单查询栏-->
            <el-row class='orderSearch'>
                <span style='margin:0 40px;font-size: 16px'>订单查询</span>
                <el-date-picker
                    v-model="timeValue"
                    type="datetimerange"
                    size='1'
                    align="right"
                    unlink-panels
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期"
                    :picker-options="pickerOptions">
                </el-date-picker>
                <el-button size='1' type='primary' style='margin-left: 20px'>查询</el-button>
                <el-button size='1' type='primary' style='float: right;margin: 5px 20px 5px 0;'>查询</el-button>
                <el-input size='1' clearable placeholder="请输入关键字查询订单" style='width: 25%;float: right;margin-right: 30px;'></el-input>
            </el-row>
            <el-row>
                <el-col class='col_top' :span="2">序号</el-col>
                <el-col class='col_top' :span="4">订单编号</el-col>
                <el-col class='col_top' :span="5">客户名称</el-col>
                <el-col class='col_top' :span="3">手机号码</el-col>
                <el-col class='col_top' :span="3">订单状态</el-col>
                <el-col class='col_top' :span="4">订单日期</el-col>
                <el-col class='col_top' :span="3">操作</el-col>
            </el-row>
            <el-row v-for="(item,index) of allOrder" :key='index'>
                <el-col :span="2">{{item.id}}</el-col>
                <el-col :span="4">{{item.orderNumber}}</el-col>
                <el-col :span="5">{{item.customerName}}</el-col>
                <el-col :span="3">{{item.phone}}</el-col>
                <el-col :span="3">{{item.orderStatus}}</el-col>
                <el-col :span="4">{{item.orderDate}}</el-col>
                <el-col :span="3"><router-link class='clickStyle' to='/ordinaryOrderDetail'>查看详情</router-link></el-col>
            </el-row>
            <!--            分页      -->
            <el-pagination
                background
                style='margin-top:15px;float: right;'
                layout="prev, pager, next"
                :total="1000">
            </el-pagination>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'ordinaryOrderList',
    data() {
        return {
            timeValue: '',
            statusList:["全部","待付款","待发货","待收货","已收货"],
            ind:0,      //对应状态数组
            pickerOptions: {
                shortcuts: [{
                    text: '本月',
                    onClick(picker) {
                        picker.$emit('pick', [new Date(), new Date()]);
                    }
                }, {
                    text: '今年至今',
                    onClick(picker) {
                        const end = new Date();
                        const start = new Date(new Date().getFullYear(), 0);
                        picker.$emit('pick', [start, end]);
                    }
                }, {
                    text: '最近六个月',
                    onClick(picker) {
                        const end = new Date();
                        const start = new Date();
                        start.setMonth(start.getMonth() - 6);
                        picker.$emit('pick', [start, end]);
                    }
                }]
            },
            allOrder:[
                {
                    id:1,
                    orderNumber:"42530002",
                    customerName:"**有限公司",
                    phone:12345546,
                    orderStatus:'待付款',
                    orderDate:'2021/02/06 18:33:45'
                },
                {
                    id:1,
                    orderNumber:"42530002",
                    customerName:"广东**有限公司",
                    phone:12345546,
                    orderStatus:'待收货',
                    orderDate:'2021/02/06 18:33:45'
                },
                {
                    id:1,
                    orderNumber:"42530002",
                    customerName:"个**有限公司",
                    phone:12345546,
                    orderStatus:'待评价',
                    orderDate:'2021/02/06 18:33:45'
                },
            ]

        }
    },
    methods:{
        selectAndChange(index,item){     //选择种类查询
            this.ind = index;
            //  alert(item);
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
.breadcrumb2{
    font-size: 17px;
    font-weight: bold;
    height: 30px;
    line-height: 30px;
    width: 50%;
}
.orderStatus span{
    margin:0 20px;
    cursor: pointer;    /*放上去边手型*/
}
.active{
    font-weight: bold;
    font-size: 17px;
    color: #e6a23c;
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
.el-col{
    height: 50px;
    line-height: 50px;
    border-bottom: 1px solid rgb(240,240,240);
}
.col_top{
    font-weight: bold;
    background-color: rgb(245,245,245);
}
</style>
