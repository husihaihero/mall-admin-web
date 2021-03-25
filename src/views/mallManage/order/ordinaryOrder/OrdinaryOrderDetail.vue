<template>
    <el-container>
        <el-header height='30%'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>订单管理</el-breadcrumb-item>
                <el-breadcrumb-item>普通订单</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>订单详情</a></el-breadcrumb-item>
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
            <!-- 收件信息 -->
            <el-row>
                <el-col :span='12' style='font-size: 16px'><b>收件人信息</b></el-col>
                <el-col :span='12' style='font-size: 20px;color: #2d8cf0;text-align:right'>
                    <i class='el-icon-edit-outline'></i>
                </el-col>
            </el-row>
            <el-divider></el-divider>  <!-- 分割线-->
            <el-row>
                <el-col :span="6"><b>收件人：</b>×××</el-col>
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
                <el-col :span='11'>
                    <span  v-if='editSelect'>
                        <el-select v-model='distributeType' placeholder='选择配送方式'>
                            <el-option
                                v-for='item of distributeOptions'
                                :key="item.value"
                                :label="item.label"
                                :value="item.value"
                                :disabled="item.disabled"
                                        >
                            </el-option>
                        </el-select>
                        <el-button type='primary' style='width: 70px' @click='editSelectConfirm()'>确认</el-button>
                    </span>
                    <span v-else>{{distributeType}}</span>
                    <span style='margin-left: 15px'>专线物流自提￥88</span>
                </el-col>
                <el-col :span='4' :offset='7' style='text-align:right'>
                    <i class='el-icon-edit-outline' @click='editSelect = true'></i>
                </el-col>
            </el-row>
            <!-- 质检报告 -->
            <el-row>
                <el-col :span='2'>
                    <b style='font-size: 16px'>质检报告</b>
                </el-col>
                <el-col :span='11'>
                    <span>申请状态：已申请</span>
                    <span style='margin-left: 15px'>申请类别：申亿质检</span>
                </el-col>
                <el-col :span='4' :offset='7' style='text-align:right'>
                   <el-link :underline='false'>上传质检报告</el-link>
                </el-col>
            </el-row>
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
    name: 'ordinaryOrderDetail',
    data(){
        return{
            editSelect:false,   //修改配送方式时的下拉框
            distributeType:"货运",   //实际配送方式
            distributeOptions:[     //配送方式的类型
                {
                    label:"货运",
                    value:"货运"
                },
                {
                    label:"货到付款",
                    value:"货到付款"
                },
                {
                    label:"自提",
                    value:"自提"
                },
                {
                    label:"快递",
                    value:"快递",
                    disabled:true
                },
            ]
        }
    },
    methods:{
        //修改确认
        editSelectConfirm(){
            this.$confirm('修改当前配送方式信息后，配送方式以更改后的为准，请您确认信息无误后保存更改', '确认保存当前配送方式吗？', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                this.$message({
                    type: 'success',
                    message: '保存成功!',
                });
                this.editSelect=false
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消',
                });
                this.editSelect=false
            });

        }
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
