<!--质检管理中订单详情页-->
<template>
    <el-container>
        <el-header height='22%'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item><a href="">订单管理</a></el-breadcrumb-item>
                <el-breadcrumb-item><a href="">质检管理</a></el-breadcrumb-item>
                <el-breadcrumb-item><a href="">订单详情</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <el-row>
                <b>订单编号:<span>1453154133</span></b>
            </el-row>
            <br />
            <el-row>
                <el-col :span="6"><b>客户名称：</b><span>申亿五金标准件有限公司</span></el-col>
                <el-col :span="6" :offset="1"><b>订购产品：</b><span>**螺栓</span></el-col>
                <el-col :span="3" :offset="1">订单状态</el-col>
                <el-col :span="3" :offset="1">订单金额</el-col>
            </el-row>
            <el-row>
                <el-col :span="6"><b>手机号码：</b><span>13531415541</span></el-col>
                <el-col :span="6" :offset="1"><b>订单日期：</b><span>2021/1/34 18:10:35</span></el-col>
                <el-col :span="3" :offset="1"><b>已收货</b></el-col>
                <el-col :span="3" :offset="1"><b>￥<span>234.00</span></b></el-col>
            </el-row>
        </el-header>
        <el-main>
            <p><b style='font-size: 16px'>收件人信息</b></p>
            <el-divider></el-divider>  <!-- 分割线-->
            <el-row>
                <el-col :span="6"><b>收件人：</b>×××</el-col>
                <el-col :span="6"><b>电话号码：</b>123××××1234</el-col>
                <el-col :span="6"><b>所在地区：</b>××省××市区</el-col>
                <el-col :span="6"><b>详细地址：</b>××××××</el-col>
            </el-row>
            <el-row>
                <el-col :span="6"><b>配送方式：</b><span>货运</span>&nbsp;&nbsp;专线物流自提￥<span>88</span></el-col>
            </el-row>
            <br />
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
            <p><b style='font-size: 16px'>商品信息</b></p>
            <br />
            <div class='productInfo'>
                <el-row style='background-color:rgb(245,245,245);border-bottom: 1px solid rgb(220,220,220)'>
                    <b style='font-size: 16px;height: 40px;line-height: 40px;'>备注：</b><span>备注内容备注内容备注内容</span>
                </el-row>
                <el-row>
                    <el-col :span="4">
                        <img src='../../../../assets/img/img.jpg' style='width:90px;height: 90px;'/>
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
    name: 'specialOrderDetail'
};
</script>

<style scoped>

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
/*.el-col{*/
/*    height: 50px;*/
/*    line-height: 50px;*/
/*    border-bottom: 1px solid rgb(240,240,240);*/
/*}*/
.el-row{
    padding-left: 10px;
    margin-bottom: 5px;
}
.invoiceInfo,.productInfo{
    border: 1px solid rgb(220,220,220);
    margin-left: 20px;
    margin-right: 20px;
}

</style>
