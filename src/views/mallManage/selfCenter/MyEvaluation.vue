<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>个人中心</el-breadcrumb-item>
                <el-breadcrumb-item>我的评价</el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>我的评价</span>
        </el-header>
        <el-main>
            <div>
                <el-select  v-model="default1">
                    <el-option
                        v-for='item of dateTime'
                        :key='item.value'
                        :label="item.label"
                        :value="item.value">
                    </el-option>
                </el-select>
                <span style='margin-left: 20px'>时间排序</span><!--  <i class='el-icon-d-caret' style='color: #2d8cf0'></i>-->
                <i class='el-icon-caret-top' @click='topShow()' :class='{iStyle: isShow}' ></i>
                <i class='el-icon-caret-bottom' @click='bottomShow()' :class='{iStyle: !isShow}' ></i>
            </div>
            <!--评论主要信息-->
            <div style='margin-top: 30px'>
                <template v-for='item of evaluations' >
                    <el-row>
                        <!--评论者头像-->
                        <el-col :span='1' style='margin-right: 5px'>
                            <el-avatar :size="50" :src="item.pic"></el-avatar>
                        </el-col>
                        <!--评论者姓名、评价星级及评论内容-->
                        <el-col :span='22'>
                            <el-row class='evaluationContent'>
                                <el-col :span='21'>
                                    <el-row>
                                        <el-col :span='2'>
                                            <b><span>{{item.name}}</span></b>
                                        </el-col>
                                        <!--星级评价-->
                                        <el-col :span='22'>
                                            <el-rate
                                                v-model='item.rateValue'
                                                disabled
                                                show-score
                                                text-color="#ff9900"
                                                score-template="{value}">
                                            </el-rate>
                                        </el-col>
                                    </el-row>
                                    <el-row>
                                        {{item.content}}
                                    </el-row>
                                </el-col>
                                <!--评论时间-->
                                <el-col :span='3' style='color: #bbbbbb;'>
                                    <span>{{item.time}}</span>&nbsp;&nbsp;&nbsp;
                                    <span>{{item.percentTime}}</span>小时前
                                </el-col>
                            </el-row>
                        </el-col >
                    </el-row>
                </template>
            </div>
            <div style='float: right'>
                <el-pagination
                    background
                    @size-change="handleSizeChange"
                    @current-change="handleCurrentChange"
                    :current-page="currentPage4"
                    :page-sizes="[100, 200, 300, 400]"
                    :page-size="100"
                    layout="total, sizes, prev, pager, next, jumper"
                    :total="400">
                </el-pagination>
            </div>
        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'MyEvaluation',
    data(){
        return{
            isShow:true,    //图标样式的显示
            default1:'1',   //默认排序选择
            currentPage4: 1,    //当前页
            dateTime:[
                {
                    value:"1",
                    label:"最近一天",
                },{
                    value:"3",
                    label:"最近三天",
                },{
                    value:"30",
                    label:"最近一个月",
                }
            ],
            evaluations:[   //评论信息
                {
                    name:"林冬冬",
                    rateValue:3.7,                       //星级
                    content:"物流很快，客服服务态度很不错",   //评论内容
                    time:"2021/6/17",                   //评价时间
                    percentTime:"5",                     //距离现在的时间
                    pic:require('../../../assets/img/img.jpg')  //评论者头像
                },
                {
                    name:"韩梅梅",
                    rateValue:4.0,
                    content:"物流很快，客服服务态度很不错",
                    time:"2021/6/17",
                    percentTime:"5",
                    pic:require('../../../assets/img/waitExplain.jpg')
                },
                {
                    name:"张三",
                    rateValue:5.0,
                    content:"物流很快，客服服务态度很不错",
                    time:"2021/6/18",
                    percentTime:"6",
                    pic:require('../../../assets/img/img.jpg')
                },
            ]
        }
    },
    methods:{
        topShow(){
            this.isShow=true;

        },
        bottomShow(){
            this.isShow=false;

        },
        handleSizeChange(val) {
            console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
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
.el-main{
    margin: 15px;
    background-color: white;
    font-size: 14px;
}
.el-icon-caret-top,.el-icon-caret-bottom{
    color: gray;
}
.iStyle{
    color: #2d8cf0;
}
.evaluationContent{
    padding-bottom:15px;
    margin-bottom:20px;
    border-bottom: 1px solid rgb(229,229,229)
}
</style>