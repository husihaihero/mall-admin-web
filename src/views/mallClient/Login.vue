<template>
    <div class="login-wrap" >
        <div class="ms-login" >
            <div class="ms-title">
                <a><span class='ms-title1' :class="{active: showLogin1}"  @click="showLoginM1()">密码登录</span></a>
                <a><span class='ms-title2' :class="{active: showLogin2}"  @click="showLoginM2()">验证码登录</span></a>
                <span style='float: right; margin-right: 35px;font-size: 18px' @click="deleteBtn()">×</span>
            </div>
            <!--  密码登录  -->
            <el-form :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content" v-show='showLogin1'>
                <el-form-item prop="username">
                    <el-input v-model="param.username" placeholder="username">
                        <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input
                        type="password"
                        placeholder="输入密码"
                        v-model="param.password"
                    >
                        <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
                    </el-input>
                </el-form-item>

                <div class="pwd">
                    <span class="pwdL"><input type='checkbox' style='vertical-align:middle' /> 记住密码</span>
                    <span class="pwdR"><a href="#" style='color: rgb(0,145,255)'>忘记密码?</a></span>
                </div>
                <div class="login-btn">
                    <el-button type="primary" @click="pwdLogin()">登录</el-button>
                </div>
                <div class='register-link'>
                    <span style='color: rgb(96,98,102)'>未注册？</span>&nbsp<router-link to='/Register'><span style='color: rgb(0,145,255)'>立即注册</span></router-link>
                </div>
            </el-form>

            <!--       验证码登录  -->
            <el-form :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content" v-show='showLogin2' >
                <el-form-item prop="cellphone">
                    <el-input  placeholder="手机号码" v-model='param.cellphone'>
                        <el-button slot="prepend" icon="el-icon-phone"></el-button>
                    </el-input>
                </el-form-item>
<!--                <el-form-item prop="imgCode" >-->
<!--                    <el-input type='text' placeholder="请输入图形码" style='width:180px;margin-right:15px'></el-input>-->
<!--                    <el-button>图形验证码</el-button>-->
<!--                </el-form-item>-->
                <el-form-item prop="phoneCode" :inline="true" >
                    <el-input type='text' placeholder="验证码" v-model='param.phoneCode' style='width:180px; margin-right:15px'></el-input>
                    <el-button type="primary" >发送验证码</el-button>
                </el-form-item>
                <div class="login-btn">
                    <el-button type="primary" @click="phoneLogin()">登录</el-button>
                </div>
                <div class='register-link'>
                    <span style='color: rgb(96,98,102)'>未注册？</span>&nbsp<router-link to='/register'><span style='color: rgb(0,145,255)'>立即注册</span></router-link>
                </div>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            showLogin1:true,
            showLogin2:false,
            loginTitle:['密码登录','短信登录'],
            currentIndex:0,
            param: {
                username: '',
                password: '',
                cellphone:'',
                phoneCode:'',
            },
            rules: {
                username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
                password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
                cellphone: [{ required: true, message: '请填写手机号', trigger: 'blur' }],
                phoneCode: [{ required: true, message: '请填写验证码', trigger: 'blur' }],
            },
        }
    },
    methods: {
        pwdLogin() {
            this.$refs.login.validate(valid => {
                if (valid) {
                    this.$message.success('登录成功');
                    localStorage.setItem('ms_username', this.param.username);
                    this.$router.push('/');
                } else {
                     //this.$message.error('请输入手机号和验证码');
                    console.log('error submit!!');
                    return false;
                }
            });
        },
        phoneLogin() {
            this.$refs.login.validate(valid2 => {
                if (valid2) {
                    this.$message.success('登录成功');
                    localStorage.setItem('ms_username', this.param.cellphone);
                    this.$router.push('/');
                } else {
                    this.$message.error('请输入手机号和验证码');
                    console.log('error submit!!');
                    return false;
                }
            });
        },
        deleteBtn(){
            alert("已退出登录页面");

        },
        showLoginM1(){
            this.showLogin1=true;
            this.showLogin2=false;
        },
        showLoginM2(){
            this.showLogin1=false;
            this.showLogin2=true;
        },

    },
};
</script>

<style scoped>

.title-ul li{
    display: inline-block;
    list-style: none;
}
.ms-title {
    float: left;
    width: 100%;
    line-height: 50px;
    text-align: center;
    font-size: 15px;
    font-family: "Microsoft YaHei";
    /*color: rgb(0,145,255);*/
    border-bottom: 1px solid rgb(230,230,232);
    background-color: white;
    margin-bottom: 20px;

}
.ms-title1 {
    float: left;
    margin-left: 33px;

}
.ms-title2 {
    float: left;
    margin-left: 15px;
}
.active {
    font-weight: bold;
    font-size: 18px;
    color:rgb(0,145,255);
}
.ms-login {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 350px;
    margin: -190px 0 0 -175px;
    border-radius: 5px;
    /*background: rgba(255, 255, 255, 0.3);*/
    overflow: hidden;
    background-color: white;
}
.ms-content {
    padding: 30px 30px;
}
.login-btn {
    text-align: center;
}
.login-btn button {
    width: 100%;
    height: 36px;
    margin-bottom: 10px;
}

.pwd{
    font-size: 14px;

}
.pwdL{
    vertical-align: -3px;
    color: rgb(96,98,102);
}
.pwdR{
    float: right;
    height: 30px;
    color: blue;
}
.register-link{
    width: 100%;
    text-align: center;
    font-size: 14px;
}
</style>