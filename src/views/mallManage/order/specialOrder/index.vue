<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>订单管理</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>非标订单</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>非标订单</span>
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
            <div class='orderList'>
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
                    <el-col :span="3" >
                        <router-link class='clickStyle' to=''>查看详情</router-link>&nbsp;&nbsp;
                        <span class='clickStyle' @click='contractConfirmDialog = true'>合同确认</span>
                    </el-col>
                </el-row>
           </div>
            <!--            分页      -->
            <el-pagination
                background
                style='margin-top:15px;float: right;'
                layout="prev, pager, next"
                :total="1000">
            </el-pagination>
        </el-main>
        <!--        合同确认弹出层-->
        <el-dialog title="合同确认" :visible.sync="contractConfirmDialog"
                   class='contractConfirm'  width="720px">
            <div class='orderInfo'>
                <el-row>
                    <el-col>订单基本信息（非标订单）</el-col>
                </el-row>
                <el-row>
                    <el-col span='10'>
                        <strong>订单编号：</strong>
                        <span>13148295632</span>
                    </el-col>
                    <el-col span='12'>
                        <strong>订单金额：</strong>￥
                        <span>234.00</span>
                    </el-col>
                </el-row>
                <el-row>
                    <el-col span='10'>
                        <strong>客户名称：</strong>
                        <span>申亿五金标准件有限公司</span>
                    </el-col>
                    <el-col span='12'>
                        <strong>订单产品：</strong>
                        <span>***螺栓加防腐处理等</span>
                    </el-col>
                </el-row>
            </div>
            <el-main>
            <!--    我方合同上传   -->
            <div >
                <p>我方合同上传</p>
                <el-upload
                    action="#"
                    list-type="picture-card"
                    :auto-upload="false">
                    <i slot="default" class="el-icon-plus"></i>
                    <div slot="file" slot-scope="{file}">
                        <img
                            class="el-upload-list__item-thumbnail"
                            :src="file.url" alt=""
                        >
                        <span class="el-upload-list__item-actions">
                             <!-- 查看 -->
                             <span
                                  class="el-upload-list__item-preview"
                                  @click="handlePictureCardPreview(file)"
                             >
                                <i class="el-icon-zoom-in"></i>
                             </span>
                             <!-- 下载 -->
                             <span
                                 v-if="!disabled"
                                 class="el-upload-list__item-delete"
                                 @click="handleDownload(file)"
                             >
                               <i class="el-icon-download"></i>
                             </span>
                             <!-- 删除-->
                             <span
                                 v-if="!disabled"
                                 class="el-upload-list__item-delete"
                                 @click="handleRemove(file)"
                             >
                               <i class="el-icon-delete"></i>
                             </span>
                      </span>
                    </div>
                </el-upload>
                <!--   我方合同大图显示    -->
                <el-dialog :visible.sync="dialogVisible" z-index='1'>
                    <img width="100%" :src="dialogImageUrl" alt="">
                </el-dialog>
            </div>

            <br />

            <!--    客户合同查看   -->
            <div >
                <p>客户合同查看</p>
                <el-upload
                    action="#"
                    list-type="picture-card"
                    :auto-upload="false">
                    <i slot="default" class="el-icon-plus"></i>
                    <div slot="file" slot-scope="{file}">
                        <img
                            class="el-upload-list__item-thumbnail"
                            :src="file.url" alt=""
                        >
                        <span class="el-upload-list__item-actions">
                             <!-- 查看 -->
                             <span
                                 class="el-upload-list__item-preview"
                                 @click="handlePictureCardPreview(file)"
                             >
                                <i class="el-icon-zoom-in"></i>
                             </span>
                            <!-- 下载 -->
                             <span
                                 v-if="!disabled"
                                 class="el-upload-list__item-delete"
                                 @click="handleDownload(file)"
                             >
                               <i class="el-icon-download"></i>
                             </span>
                            <!-- 删除-->
                             <span
                                 v-if="!disabled"
                                 class="el-upload-list__item-delete"
                                 @click="handleRemove(file)"
                             >
                               <i class="el-icon-delete"></i>
                             </span>
                      </span>
                    </div>
                </el-upload>
                <!--    客户合同大图显示    -->
                <el-dialog :visible.sync="dialogVisible">
                    <img width="100%" :src="dialogImageUrl" alt="">
                </el-dialog>
            </div>
            <br /><p>合同确认(信息确认无误后将生成购物订单)</p><br />

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
                <el-button @click='contractConfirmDialog = false'>取消</el-button>
            </div>
            </el-main>
        </el-dialog>
    </el-container>
</template>

<script>
export default {
    name: 'specialOrderList',
    data() {
        return {
            timeValue: '',      //搜索时间值
            contractConfirmDialog:false,    //合同确认弹出框
            dialogImageUrl: '', //下面三个都是上传文件组件自带的数值
            dialogVisible: false,
            disabled: false,
            info:null,     //判断信息是否正确
            statusList:["全部","合同待确认","待付款","生产中","待发货","待收货","已收货"],
            ind:0,      //对应状态数组
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
                    orderStatus:'待付款',
                    orderDate:'2021/02/06 18:33:45'
                },
                {
                    id:1,
                    orderNumber:"42530002",
                    customerName:"广东**有限公司",
                    phone:12345546,
                    orderStatus:'合同待确认',
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
.orderSearch{
    width: 100%;
    height: 50px;
    line-height: 50px;
    margin-top: 10px;
    margin-bottom: 10px;
    font-weight: bold;
    border:1px solid rgb(240,240,240);
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
.el-main .orderList .el-col{
    height: 50px;
    line-height: 50px;
    border-bottom: 1px solid rgb(240,240,240);
}
.col_top{
    font-weight: bold;
    background-color: rgb(245,245,245);
}
.el-dialog__wrapper >>> .el-dialog__header{
    border-bottom: 1px solid #e8eaec;
    font-size: 16px;
    font-weight: bold;
}
.el-dialog__wrapper >>> .el-dialog__body{
    padding: 5px 0;
    font-size: 16px;
}
.contractConfirm .el-col{
    padding-left: 15px;
}
.orderInfo{
    background-color: rgb(233,233,233);
}
.orderInfo .el-row{
    color: black;
    margin-bottom: 10px;
}
.modal{
    background-color: white;
    opacity: 0.5;
}

</style>
