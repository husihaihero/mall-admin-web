<template>
    <el-container>
        <el-header height='30%'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>客户管理</a></el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>详情</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>详情</span>
            <br/><br/>
        </el-header>
        <el-main>
            <el-row>
                <el-col :span='5'>
                    <span>{{enterpriseInfo.name}}</span>
                </el-col>
                <el-col :span='4'>
                    <span><img>认证状态：{{enterpriseInfo.status}}</span>
                </el-col>
                <el-col :span='15' style='text-align: right'>
                    <span>提交时间：{{enterpriseInfo.commitTime}}</span>
                </el-col>
            </el-row>
            <div class='mainInfo'>
                <ul>
                    <li>企业全称：{{enterpriseInfo.enterpriseName}}</li>
                    <li>营业执照：<el-link @click='licenseDialog = true' type='primary' :underline='false'>点击查看</el-link></li>
                    <li>所在地址：{{enterpriseInfo.address}}</li>
                    <li>注册地址：{{enterpriseInfo.registerAddress}}</li>
                </ul>
                <ul>
                    <li>纳税人编码：{{enterpriseInfo.taxer_code}}</li>
                    <li>
                        申办人(代理人)：{{enterpriseInfo.agent}}
                        <el-link @click='letterDialog = true' type='primary' :underline='false'> 查看委托书</el-link>
                    </li>
                    <li>手机号码：{{enterpriseInfo.phone}}</li>
                    <li>身份证号：{{enterpriseInfo.idCard}}</li>
                </ul>
                <ul>
                    <li style='width: 100%;border-bottom:2px solid silver;'><span>账户信息</span>
                    {{enterpriseInfo.accountNumber}} ({{enterpriseInfo.accountNumber}})
                    </li>
                </ul>
            </div>

            <!--   查看营业执照   -->
            <el-dialog :visible.sync="licenseDialog" >
                <el-row style='font-size: 18px'>
                    <el-col :span='12' style='text-align: left' >
                        <i  @click="handlePictureCardPreview(enterpriseInfo.licensePictureAddress)" class='el-icon-zoom-in'></i>
                        <i class='el-icon-zoom-out'></i>
                    </el-col>
                    <el-col :span='12' style='text-align: right'>
                        <el-link :underline='false' style='font-size: 18px' @click='licenseDialog=false'>×</el-link>
                    </el-col>
                </el-row>
                <el-divider></el-divider>
                <el-image :src='enterpriseInfo.licensePictureAddress' width='800px' height='550px'></el-image>
            </el-dialog>
            <!--   营业执照大图  -->
            <el-dialog :visible.sync="dialogVisible">
                <img width="100%" :src="dialogImageUrl" alt="">
            </el-dialog>

            <!--   查看委托书   -->
            <el-dialog :visible.sync="letterDialog">
                <el-row style='font-size: 18px'>
                    <el-col :span='12' style='text-align: left'>
                        <i class='el-icon-zoom-in'></i>
                        <i class='el-icon-zoom-out'></i>
                    </el-col>
                    <el-col :span='12' style='text-align: right'>
                        <el-link :underline='false' style='font-size: 18px' @click='letterDialog=false'>×</el-link>
                    </el-col>
                </el-row>
                <el-divider></el-divider>
                <img :src='enterpriseInfo.LetterPictureAddress' />
            </el-dialog>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'customerDetail',
    data(){
        return{
            licenseDialog:false,       //营业执照隐藏/展示
            letterDialog:false,     //委托书隐藏/展示
            dialogVisible:false,      //图片大图隐藏/展示
            dialogImageUrl: '',  //图片大图地址
            enterpriseInfo:{
                name:"湖南申亿五金标准件有限公司",
                status:"企业",
                commitTime:"2020/11/23 13:30:22",
                enterpriseName:"湖南申亿五金标准件有限公司",
                licensePictureAddress:"https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg",
                address:"湖南省长沙市天心区",
                registerAddress:"湖南省长沙市天心区",
                taxer_code:"123456",
                agent:"张三",
                LetterPictureAddress:"https://fuss10.elemecdn.com/1/34/19aa98b1fcb2781c4fba33d850549jpeg.jpeg",
                phone:"15523542456",
                idCard:"43219389938283838",
                accountNumber:"12929320398398490",
                accountBank:"湖南省长沙市天心区中国银行长沙分行",
            }
        }
    },
    methods:{
        handlePictureCardPreview(e){
            this.dialogImageUrl = e;
            this.dialogVisible = true;
        }
    }
};
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

.mainInfo .el-col{
    height: 40px;
    line-height: 40px;

}
.mainInfo ul {
    margin: 0;
    padding: 0;
    display: flex;
    width: 100%;
    height:40px;
    border: 1px solid silver;
    border-bottom: none;
}
.mainInfo ul li{
    list-style: none;
    width: 25%;
    height:40px;
    line-height: 40px;
    padding-left: 5px;
    border: 1px solid silver;
}
/deep/ .el-dialog{
    width: 840px;
}
/deep/ .el-dialog__header{
    display: none;
}
/deep/ .el-dialog__body{
    padding: 15px 20px;

}
/deep/ .el-divider{
    margin-top: 5px;
}
</style>
