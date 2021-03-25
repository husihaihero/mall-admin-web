<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">
                <a><span class='ms-title1'>账号注册</span></a>
                <span style='float: right; margin-right: 35px;font-size: 18px' @click="deleteBtn()">×</span>
            </div>

            <!--     账号注册   -->
            <el-form :model="param" :rules="rules" ref="register" label-width="0px" class="ms-content"  >

                <el-form-item prop="username">
                    <span>用户姓名</span>
                    <span class='fix-flag'>*</span>
                    <el-input class='input-width' v-model='param.username' placeholder="请输入您的姓名" ></el-input>
                </el-form-item>
                <el-form-item prop="cellphone">
                    <span>手机号码</span>
                    <span class='fix-flag'>*</span>
                    <el-input class='input-width' v-model='param.cellphone'  placeholder="请输入您的手机号码"></el-input>
                </el-form-item>
                <el-form-item prop="phoneCode" :inline="true" >
                    <span>手机验证</span>
                    <span class='fix-flag'>*</span>
                    <el-input type='text' v-model='param.phoneCode' placeholder="请输入手机验证码" style='width:140px; margin-left:15px; margin-right:15px'></el-input>
                    <el-button type="primary" >发送验证码</el-button>
                </el-form-item>
                <el-form-item prop="password">
                    <span>登录密码</span>
                    <span class='fix-flag'>*</span>
                    <el-input class='input-width' v-model='param.password'  placeholder="请设置登录密码"></el-input>
                </el-form-item>
                <el-form-item prop="confirmPwd">
                    <span>确认密码</span>
                    <span class='fix-flag'>*</span>
                    <el-input class='input-width' v-model='param.confirmPwd' placeholder="请再次输入密码"></el-input>
                </el-form-item>
                <el-form>
                    <el-checkbox></el-checkbox>
                    <el-text style='font-size: 14px;'>
                        <a href='#' style='color: rgb(0,145,255)'>《申亿五金标准件有限公司条款》</a> 我已阅读并接受
                    </el-text>
                </el-form>
                <br>
                <el-form class="login-btn">
                    <el-button type="primary" @click="submitForm()">注册</el-button>
                </el-form>

            </el-form>
            <!--    注册成功界面    -->
            <div class='successReg' v-show='successReg' :append-to-body="true" >
                <br>
                <div><img src='../../assets/img/success.png'></div>
                <div style='font-family: "Microsoft YaHei";font-weight: bold;margin-bottom: 20px;'>注册成功</div>
                <el-form class="login-btn">
                    <router-link to='/userLogin'><el-button type="primary" style='width:290px'>前往登录</el-button></router-link>
                </el-form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            successReg:false,
            currentIndex:0,
            param: {
                username:'',
                cellphone:'',
                phoneCode:'',
                password:'',
                confirmPwd:''
            },
            rules: {
                username: [{ required: true, message: '请填写您的姓名', trigger: 'blur' }],
                cellphone: [{ required: true, message: '请填写手机号码', trigger: 'blur' }],
                phoneCode: [{ required: true, message: '请填写验证码', trigger: 'blur' }],
                password: [{ required: true, message: '请填写密码', trigger: 'blur' }],
                confirmPwd: [{ required: true, message: '请再填写密码', trigger: 'blur' }],

            },
        };
    },
    methods: {

        submitForm() {
            this.successReg=!this.successReg;
            this.$refs.login.validate(valid => {
                if (valid) {
                    this.$message.success('注册成功');
                    localStorage.setItem('ms_username', this.param.username);
                    this.$router.push('/');
                } else {
                    this.$message.error('');
                    console.log('error submit!!');
                    return false;
                }
            });
        },
        deleteBtn(){
            alert("已退出登录页面");

        },

    },
};
</script>

<style scoped>
/*.login-wrap {*/
/*    position: relative;*/
/*    width: 100%;*/
/*    height: 100%;*/
/*    background-image: url(../../assets/img/login-bg.jpg);*/
/*    background-size: 100%;*/
/*}*/
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
    font-weight: bold;
    font-size: 18px;
    color:rgb(0,145,255);
}

.ms-login {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 400px;
    margin: -190px 0 0 -175px;
    border-radius: 5px;
    /*background: rgba(255, 255, 255, 0.3);*/
    overflow: hidden;
    background-color: white;
}
.ms-content {
    padding: 30px 30px;


}
.fix-flag{
    color: red;
}
.login-btn button {
    width: 100%;
    height: 36px;
    margin-bottom: 10px;
}
.input-width {
    width:250px;
    margin-left:15px;
}
.successReg{
    position: absolute;
    left: 50%;
    top: 50%;
    width:320px;
    height: 210px;
    margin: -120px 0 0 -155px;
    border-radius: 5px;
    text-align: center;
    overflow: hidden;
    background-color: white;
}
</style>
