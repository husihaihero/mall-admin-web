<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>个人中心</el-breadcrumb-item>
                <el-breadcrumb-item>账户信息设置</el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>账户信息设置</span>
        </el-header>

        <el-main>
            <el-tabs tab-position="left" >
                <el-tab-pane label="基本设置" style='font-size: 24px'>
                    <p style='font-size: 18px;font-weight: bold;margin-bottom: 10px'>基本设置</p>
                    <el-form ref="form" label-position="top" :model="form">
                        <div style='margin-left: 65%;position:absolute;'>
                            <p>头像</p>
                            <el-avatar :size="100" :src="form.pic"></el-avatar>
                            <br/>
                            <el-button style='margin-left: 10px'>更换头像</el-button>
                        </div>
                        <el-form-item label="邮箱">
                            <el-input v-model="form.mailbox" style='width:500px' clearable></el-input>
                        </el-form-item>
                        <el-form-item label="姓名">
                            <el-input v-model="form.name" style='width:500px' clearable></el-input>
                        </el-form-item>
                        <el-form-item label="所在省市">
                            <el-select v-model='defaultProvinces'>
                                <el-option
                                    v-for="item in provinces"
                                    :key="item.value"
                                    :label="item.label"
                                    :value="item.value">
                                </el-option>
                            </el-select>
                            <el-select v-model='defaultCity'>
                                <el-option
                                    v-for="item in cities"
                                    :key="item.value"
                                    :label="item.label"
                                    :value="item.value">
                                </el-option>
                            </el-select>

                        </el-form-item>
                        <el-form-item label="详细地址">
                            <el-input v-model="form.detailAddress" style='width:500px' clearable></el-input>
                        </el-form-item>
                        <el-form-item label="手机号码">
                            <el-input v-model="form.phone" style='width:400px' clearable></el-input>
                        </el-form-item>
                        <el-button type='primary'>更新基本信息</el-button>
                    </el-form>
                </el-tab-pane>

                <el-tab-pane label="安全设置">
                    <p style='font-size: 18px;font-weight: bold;margin-bottom: 10px'>基本设置</p>
                    <el-row style='font-size: 16px;font-weight: bold;margin-bottom: 10px'>账户密码</el-row>
                    <el-row style='color:darkgray '>
                        <el-col :span='22'>当前密码强弱：<span>强</span></el-col>
                        <el-col :span='2'><el-link style='color: #2d8cf0'>修改</el-link></el-col>
                    </el-row>
                </el-tab-pane>
            </el-tabs>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'UserInfoSetting',
    data(){
        return{
            form: {
                mailbox: '159409@qq.com',
                name: '麻',
                province: '',
                city: '',
                detailAddress: '雨花区',
                phone: '13122334455',
                pic:require('../../../assets/img/waitExplain.jpg')
            },
            defaultProvinces:"湖南省",    //默认省份
            defaultCity:"长沙市",     //默认城市
            provinces:[
                {
                    value:"湖南省",
                    label:"湖南省",
                },{
                    value:"湖北省",
                    label:"湖北省",
                },{
                    value:"四川省",
                    label:"四川省",
                },{
                    value:"重庆市",
                    label:"重庆市",
                },{
                    value:"广东省",
                    label:"广东省",
                },
            ],
            cities:[
                {
                    value:"长沙市",
                    label:"长沙市",
                },{
                    value:"永州市",
                    label:"永州市",
                },
                {
                    value:"岳阳市",
                    label:"岳阳市",
                },
                {
                    value:"常德市",
                    label:"常德市",
                },
            ]
        }
    },
    methods: {
        handleOpen(key, keyPath) {
            console.log(key, keyPath);
        },
        handleClose(key, keyPath) {
            console.log(key, keyPath);
        },
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
.el-main{
    margin: 15px;
    background-color: white;
    font-size: 14px;
}
.el-main el-tabs tabs__content{
    margin:15px;
}
.el-form-item el-input{
    width: 200px;
}
.el-tabs .el-tab-pane{
    margin-left: 15px;
}
/deep/ .el-tabs .el-tabs__item{
    text-align: left;
    width: 200px;
    font-weight: bold;

}
/deep/ .el-tabs .el-tabs__item.is-active{   /*修改被选中时的样式*/
    background-color: rgb(231,247,255);
}
</style>