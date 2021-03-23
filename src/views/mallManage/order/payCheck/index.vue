<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>订单管理</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>支付审核</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <b style='font-size: 18px;'>支付审核</b>
        </el-header>
        <el-main>
            <div class="orderStatus">
                <template v-for="(item,index) of statusList">
                    <span @click='selectAndChange(index,item)' :class='{active:ind == index}'>{{item}}</span>
                    <el-divider direction='vertical'></el-divider>
                </template>
            </div>

            <!--      订单查询栏     -->
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
                <el-col :span="3">
                    <router-link  class='clickStyle' to='/putCheckOrderDetail'>查看详情</router-link>&nbsp;&nbsp;
                    <span class='clickStyle' @click='checkDialog = true'>审核</span>
                </el-col>
            </el-row>
            <!--       分页      -->
            <el-pagination
                background
                style='margin-top:15px;float: right;'
                layout="prev, pager, next"
                :total="1000">
            </el-pagination>
        </el-main>
        <!--   审核    -->
        <el-dialog :visible.sync="checkDialog" title='审核'>
            <div class='orderInfo'>
                <el-row>
                    <el-col>订单基本信息（非标订单）</el-col>
                </el-row>
                <el-row>
                    <el-col span='12'>
                        <strong>订单编号：</strong>
                        <span>13148295632</span>
                    </el-col>
                    <el-col span='12'>
                        <strong>订单金额：</strong>￥
                        <span>234.00</span>
                    </el-col>
                </el-row>
                <el-row>
                    <el-col span='12'>
                        <strong>客户名称：</strong>
                        <span>申亿五金标准件有限公司</span>
                    </el-col>
                    <el-col span='12'>
                        <strong>订单产品：</strong>
                        <span>***螺栓加防腐处理等</span>
                    </el-col>
                </el-row>
            </div>
            <el-row style='font-size: 16px'>
                <el-col :span='12'>
                    <span style='color: #8c939d'>订单支付凭证</span>
                </el-col>
                <el-col :span='12' style='text-align: right'>
                    <i class='el-icon-zoom-in'></i>&nbsp;
                    <i class='el-icon-zoom-out'></i>&nbsp;
                    <i class='el-icon-refresh-left'></i>&nbsp;
                </el-col>
            </el-row>
            <el-image src="https://fuss10.elemecdn.com/1/34/19aa98b1fcb2781c4fba33d850549jpeg.jpeg" width='800px' height='550px'></el-image>
            <br/> <br/>
            <!--信息结果的确认-->
            <div>
                <el-row style='height:32px;line-height:32px'>
                    <el-col :span='10'>
                        <b style='color: #2d8cf0;'>结果确认:</b>&nbsp;&nbsp;
                        <span><el-radio v-model="info" label='1'>信息无误</el-radio ></span>
                    </el-col>
                    <el-col :span='14' style='text-align: right'>
                        <el-radio v-model="info" label='0'>信息有误</el-radio>
                        <el-input style='width: 250px' placeholder='输入原因进行反馈'></el-input>
                    </el-col>
                </el-row>
            </div>
            <el-divider></el-divider>
            <!--    确定和取消   -->
            <div style='text-align: right;margin-right: 20px'>
                <el-button type="primary">确定</el-button>
                <el-button @click='checkDialog = false'>取消</el-button>
            </div>
        </el-dialog>
    </el-container>
</template>

<script>
export default {
    name: 'payCheck',
    data() {
        return {
            checkDialog: false, // 控制审核dialog的隐藏/显示
            timeValue: '',      //搜索时间值
            contractConfirmDialog:false,    //合同确认弹出框
            dialogImageUrl: '', //下面三个都是上传文件组件自带的数值
            dialogVisible: false,
            disabled: false,
            info:1,     //判断信息是否正确
            pickerOptions: {    //el选择日期组件自带
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
                    orderStatus:'待审核',
                    orderDate:'2021/02/06 18:33:45'
                },
                {
                    id:1,
                    orderNumber:"42530002",
                    customerName:"广东**有限公司",
                    phone:12345546,
                    orderStatus:'已审核',
                    orderDate:'2021/02/06 18:33:45'
                },
                {
                    id:1,
                    orderNumber:"42530002",
                    customerName:"个**有限公司",
                    phone:12345546,
                    orderStatus:'待审核',
                    orderDate:'2021/02/06 18:33:45'
                },
            ],
            ind:0,      //对应状态数组
            statusList:["全部","线下审核","账期审核"],

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
    margin-top: 15px;
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
.orderStatus span{
    margin:0 20px;
    cursor: pointer;    /*放上去边手型*/
}
.active{
    font-weight: bold;
    font-size: 17px;
    color: #e6a23c;
}
.modal{
    background-color: white;
    opacity: 0.5;
}

/deep/ .el-dialog{
    width: 600px;
}
/deep/ .el-dialog__header{
    font-weight: bold;
    border-bottom: 1px solid darkgray;
}
/deep/ .el-dialog__body{
    padding: 15px 20px;

}
.orderInfo{
    background-color: rgb(233,233,233);
}
.orderInfo .el-row{
    color: black;
    margin-bottom: 10px;
    padding-left: 10px;
}
</style>
