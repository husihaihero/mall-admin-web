<template>
    <el-container>
        <el-header height='30%'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>订单管理</el-breadcrumb-item>
                <el-breadcrumb-item>开票管理</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>订单详情</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <el-row>
                <b>订单编号:<span>{{orderInfo.orderCode}}</span></b>
            </el-row>
            <br />
            <!-- 订单信息 -->
            <el-row>
                <el-col :span="6"><b>客户名称：</b><span>{{orderInfo.customerName}}</span></el-col>
                <el-col :span="6" :offset="1"><b>订购产品：</b><span>{{orderInfo.commodity}}</span></el-col>
                <el-col :span="3" :offset="1">订单状态</el-col>
                <el-col :span="3" :offset="1">订单金额</el-col>
            </el-row>
            <el-row>
                <el-col :span="6"><b>手机号码：</b><span>{{orderInfo.phone}}</span></el-col>
                <el-col :span="6" :offset="1"><b>订单日期：</b><span>{{orderInfo.orderTime}}</span></el-col>
                <el-col :span="3" :offset="1"><b>{{orderInfo.status}}</b></el-col>
                <el-col :span="3" :offset="1"><b>￥<span>{{orderInfo.total}}</span></b></el-col>
            </el-row>
        </el-header>
        <el-main>
            <!-- 收件信息 -->
            <el-row>
                <el-col style='font-size: 16px'><b>收件人信息</b></el-col>
            </el-row>
            <el-divider></el-divider>  <!-- 分割线-->
            <el-row>
                <el-col :span="6"><b>收件人：</b>{{receiverInfo.receiver}}</el-col>
                <el-col :span="6"><b>电话号码：</b>123××××1234</el-col>
                <el-col :span="6"><b>所在地区：</b>××省××市区</el-col>
                <el-col :span="6"><b>详细地址：</b>××××××</el-col>
            </el-row>
            <br />
            <!-- 配送方式 -->
            <el-row>
                <el-col :span='2'>
                    <b style='font-size: 16px'>配送方式</b>
                </el-col>
                <el-col :span='15'>
                    <span v-bind=distributeType>{{distributeType}}</span>
                    <span style='margin-left: 15px'>专线物流自提￥88</span>
                </el-col>
            </el-row>
            <!-- 质检报告 -->
            <br />
            <!-- 开票信息 -->
            <p><b style='font-size: 16px'>开票信息</b></p>
            <el-divider></el-divider>  <!-- 分割线-->
            <div class='invoiceInfo'>
                <el-row style='background-color:rgb(245,245,245);border-bottom: 1px solid rgb(220,220,220)'>
                    <b style='font-size: 16px;height: 40px;line-height: 40px;'>基本信息</b>
                </el-row>
                <el-row>
                    <b>发票类型：<span style='color: #2d8cf0'>增值税发票</span></b>
                </el-row>
                <el-row>
                    <el-col :span="8"><b>公司名称：</b>×××</el-col>
                    <el-col :span="8"><b>纳税人号：</b>222××××××</el-col>
                    <el-col :span="8"><b>手机号码：</b>123××××1234</el-col>
                </el-row>
                <el-divider></el-divider>
                <el-row>
                    <b style='font-size: 16px'>寄送地址:</b>
                </el-row>
                <el-row>
                    <el-col :span="6"><b>收件人：</b>×××</el-col>
                    <el-col :span="6"><b>电话号码：</b>123××××1234</el-col>
                    <el-col :span="6"><b>所在地区：</b>××省××市区</el-col>
                    <el-col :span="6"><b>详细地址：</b>××××××</el-col>
                </el-row>
            </div>
            <br />
            <!-- 商品信息 -->
            <p><b style='font-size: 16px'>商品信息</b></p>
            <br />
            <div class='productInfo'>
                <el-row style='background-color:rgb(245,245,245);border-bottom: 1px solid rgb(220,220,220)'>
                    <b style='font-size: 16px;height: 40px;line-height: 40px;'>备注：</b><span>备注内容备注内容备注内容</span>
                </el-row>
                <el-row>
                    <el-col :span="4">
                        <img src='' style='width:90px;height: 90px;' alt='商品图片'/>
                    </el-col>
                    <el-col :span="7" style='margin-top: 30px'>
                        <b>U形螺栓JB/ZQ 1234- -2000 108粗牙/黄锌</b>
                        <br>
                        <b>物料编号：</b><span>892452</span>
                    </el-col>
                    <el-col :span="4" style='margin-top: 40px'>×××/千支</el-col>
                    <el-col :span="4" style='margin-top: 40px'>××××千支</el-col>
                    <el-col :span="5" style='margin-top: 30px'>
                        <b>总金额</b><span></span>
                        <br>
                        含运费<span>10.00</span>
                    </el-col>
                </el-row>
            </div>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'invoiceDetail',
    data(){
        return{
            distributeType:"货运",   //实际配送方式
            orderInfo:[{        //订单信息
                orderCode:131485694523,
                customerName:"申亿五金标准件有限公司",
                phone:13345511223,
                commodity:"**螺栓",
                orderTime:"2021/02/22 18:30:50",
                status:"已收货",
                total:234.00,
            }],
            receiverInfo:[{
                receiver:"曲丽丽",
                phone:13452163655,
                district:"湖南长沙",
                detailDistrict:"雨花区***街道",
            }],
            invoiceInfo:[{     //发票基本信息
                invoiceType:"增值税专用发票",
                enterpriseName:"审议五金标准件有限公司",
                taxerCode:"12311455",
                cellphone:"13452163655",
                accountBank: "长沙银河天心区**分行",
                accountNumber: "23512345464512",
            }],
            commodityInfo:[{
                material:"U形螺栓JB/ZQ 1234- -2000 108粗牙/黄锌",
                materialCode:"82938455",
                price:"500",
                quantity:123,
                transportPrice:10.00,
                total:234.00,
                remarks:"备注备注备注备注备注",
            }]

        }
    },
    methods:{

    }
};
</script>

<style scoped>

.el-header {
    background-color: white;
}
.el-header .el-row{
    padding-left: 10px;
    margin-bottom: 5px;
}
.el-breadcrumb{
    margin-top: 10px;
}
.el-main{
    margin: 15px;
    background-color: white;
    font-size: 14px;
}
.el-main .el-row{
    padding-left: 10px;
    margin-bottom: 15px;
}
.invoiceInfo,.productInfo{
    border: 1px solid rgb(220,220,220);
    margin-left: 20px;
    margin-right: 20px;
}
i{
    font-size: 20px;
    color: #2d8cf0;
    cursor: pointer;
}
</style>
