<template>
    <el-container>
        <el-header height='30%'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>客户管理</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>客户管理</span>
            <el-row style='width:100%;text-align: center;padding:5px'>
                <el-input style='width: 500px;'
                          size='1'
                          placeholder='请输入关键字搜索'
                >
                </el-input>
                <el-button type='primary' size='1'>搜索</el-button>
            </el-row>
            <div class="customerType">
                <template v-for="(item,index) of customerTypeList">
                    <a @click='showCustomer(index,item)'><el-col :span='6' :class="{colActive:ind == index}">{{item}}</el-col></a>
                </template>
            </div>
        </el-header>
        <el-main>
            <div>
                <el-row>
                    <el-col class='col_top col_first' :span="2">序号</el-col>
                    <el-col class='col_top' :span="5">客户名称</el-col>
                    <el-col class='col_top' :span="4">账号类型</el-col>
                    <el-col class='col_top' :span="4">手机号码</el-col>
                    <el-col class='col_top' :span="4">法人/代理人</el-col>
                    <el-col class='col_top' :span="5">操作</el-col>
                </el-row>
                <el-row v-for="(item,index) of allUserList" :key='index'>
                    <el-col :span="2" class='col_first'>{{item.id}}</el-col>
                    <el-col :span="5">{{item.username}}</el-col>
                    <el-col :span="4">{{item.userType}}</el-col>
                    <el-col :span="4">{{item.phone}}</el-col>
                    <el-col :span="4">{{item.agent}}</el-col>
                    <el-col  :span="5">
                        <router-link class='clickStyle' to='/customerDetail'>查看</router-link>&nbsp;&nbsp;
                        <router-link class='clickStyle' to=''>修改</router-link>&nbsp;&nbsp;
                        <span class='clickStyle' @click='paySetDialog = true'>付款设置</span>
                    </el-col>
                </el-row>
           </div>
            <br/>
            <!--        分页      -->
            <el-pagination
                background
                style='float: right;'
                layout="prev, pager, next"
                :total="1000">
            </el-pagination>
        </el-main>

<!--        设置付款弹出层-->
        <el-dialog title="付款设置" :visible.sync="paySetDialog"
                   class='header-title'   width="370px">
            <p>请选择付款方式：</p>
            <el-checkbox-group v-model='payTypeList' style='margin: 15px 0;text-align: center'>
                <el-checkbox label="按期付款"></el-checkbox>
                <el-checkbox label="线下转账"></el-checkbox>
                <el-checkbox label="微信支付"></el-checkbox>
            </el-checkbox-group>
            <div style='width:100%;text-align:center;'>
                <el-button type="primary">确定</el-button>
                <el-button type="primary" @click='paySetDialog = false'>取消</el-button>
            </div>
        </el-dialog>
    </el-container>
</template>

<script>
export default {
  name: 'customerList',
    data() {
        return {
            isShow:true,
            payTypeList:["按期付款"],  //选择支付方式
            paySetDialog:false,
            customerTypeList:["全部","待审核","企业账号","个人账号"],
            ind:0,  //关联状态样式
            allUserList:[       //所有用户
                {
                    id:1,
                    username:"张三",
                    userType:"个人认证",
                    phone:12345546,
                    agent:'无',
                },
                {
                    id:2,
                    username:"李四",
                    userType:"待审核",
                    phone:1555231,
                    agent:'无',
                },
                {
                    id:3,
                    username:"王五",
                    userType:"企业认证",
                    phone:133331,
                    agent:'代理人',
                }
            ],

        };
    },
    methods: {
        handleClick(tab, event) {
            console.log(tab, event);
        },
        choose(e){
            alert(e.target);
        },
        showCustomer(index,item) {
            this.ind = index;
        }
    }

}
</script>
<style scoped>
.content{
    padding: 0;
    font-size: 16px;
    position: absolute;
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
.customerType .colActive{
    border-bottom: 2px solid #2d8cf0;
    color: #2d8cf0;
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
.el-dialog__wrapper >>> .el-dialog__header{
    border-bottom: 1px solid #e8eaec;
    font-size: 16px;
    font-weight: bold;
}
.el-dialog__wrapper >>> .el-dialog__body{
    padding: 20px;
}

</style>
