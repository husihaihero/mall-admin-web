<template>
    <el-container>
        <el-header height='180px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>个人中心</el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold;margin-bottom: 10px'>个人中心</span>
            <!--  header中的个人信息  -->
            <div class='self'>
                <div class='selfLeft'>
                    <el-avatar :size="70" :src="require('../../../assets/img/waitExplain.jpg')"></el-avatar>
                </div>
                <div class='selfLeft' style='margin:10px 0px 10px 20px;'>
                    <span><b style='font-size: 18px'>用户姓名</b></span>
                    <br><br>
                    <span style='color: #666666'>技术客服<el-divider direction="vertical"></el-divider>1312345875 </span>
                    <router-link to='/userInfoSetting' style='color: rgb(8,149,255)'><i class="el-icon-edit-outline" ></i>&nbsp;&nbsp;<span>账户信息设置</span></router-link>
                </div>
                <div class='selfRight'>
                    <el-row>
                        <el-col :span='12' ><router-link to='/myLeavingMessage' style='color:rgb(0,145,255);font-size: 16px;'>我的留言</router-link></el-col>
                        <el-col :span='12' ><router-link to="/myEvaluation" style='color:orange;font-size: 16px;'>我的评价</router-link></el-col>
                    </el-row>
                    <el-row>
                        <el-col :span='12'>28</el-col>
                        <el-col :span='12'>66</el-col>
                    </el-row>
                </div>
            </div>
        </el-header>
        <el-main>
            <el-menu default-active="1" class="el-menu-demo" mode="horizontal">
                <el-menu-item index="1">待处理</el-menu-item>
            </el-menu>
            <el-row class='messages' v-for='(item,index) of messageInfo' :key='index'>
                <el-row>
                    <el-col :span='22'>
                        <span>{{item}}</span>
                        <div style='font-size: 12px;color:darkgray'>
                            <span>2020-10-1 18.00</span>&nbsp;&nbsp;&nbsp;
                            <el-tag type="info">未查看</el-tag>
                        </div>
                    </el-col >
                    <el-col :span='2'><el-button size='mini' type='primary'>前往处理</el-button></el-col>
                </el-row>
                <el-divider></el-divider>
            </el-row>
            <div style='text-align: center'>
                <el-button size='large'>加载更多</el-button>
            </div>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'SelfCenter',
    data(){
        return{
            messageInfo:["来新的订单了","来新的订单了","来新的订单了"]
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
.self{
    margin-top: 20px;
}
.selfLeft{
    float: left;
    font-size: 14px;
}
.selfRight{
    float:right;
    width:200px;
    text-align: center;
    font-size: 24px;
}
.el-main{
    margin: 15px;
    padding: 15px;
    background-color: white;
    font-size: 14px;
}
.messages{
    font-size: 16px;
    margin: 15px;
    padding-left: 10px;
}
</style>