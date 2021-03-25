<template>
    <div >
        <div class='search-header'>
            <div style='display:inline-block;height: 40px;line-height: 40px'>
               <img style='vertical-align:middle;width: 40px;height: 40px;' src='../../assets/img/success.png' title='申亿商标' />
            </div>
            <div class='search-input'>
               <span >产品分类选择 <span> < </span></span>
               <el-input type='text' class='search-btn' placeholder="请输入关键词" style='width: 250px;height: 20px'></el-input>
                <el-button type='primary'>搜索</el-button>
            </div>

            <div class='show-user'>
              <span><img @click='showPerson()' title='头像'></span>
              <span style='color: rgb(0,145,255)' @click='showPerson()'> 1314458 </span>
              <!--  下拉信息  -->
              <span @click='showPerson()'> < </span>
            </div>
        </div>
        <div class='personal-content' v-show='personShow'>
                <div  class='personal-top'>
                    <span style='margin-left: 20px;'>用户：1314458</span>
                    <span style='font-size:24px;margin-left: 190px' @click='personShowClose()'>×</span>
                </div>
                <div class='personal-middle'>
                    <table>
                        <tr style='height: 80px'>
                            <td style='width: 90px'><img title='图1'></td>
                            <td style='width: 90px'>图标2</td>
                            <td style='width: 90px'>图标3</td>
                        </tr>
                        <tr style='height: 50px'>
                            <td>个人中心</td>
                            <td>我的订单</td>
                            <td>我的收藏</td>
                        </tr>
                    </table>
                </div>
                <div class='personal-footer'>
                    <a>退出登录</a>
                </div>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            personShow:false,
        };
    },
    methods: {
        showPerson(){
            this.personShow=!this.personShow;
        },
        personShowClose(){
            this.personShow=false;
        }

    },
};
</script>

<style scoped>

.search-header{
    margin: 0 auto;
    padding: 0;
    text-align: center;
    width:100%;
    height: 50px;
    line-height: 50px;
    background-color: white;
}

.search-input{
    display: inline-block;
    height: 32px;
    line-height: 32px;
    font-size: 14px;
    color: rgb(216,216,216);
    margin:0 20%;
}
.show-user{
    display: inline-block;
}
.personal-content{
        position: absolute;
        right: 10%;
        top:11%;
        margin: 0 auto;
        width: 380px;
        height: 250px;
        background-color: white;

}
.personal-top{
    text-align: left;
    height: 50px;
    line-height: 50px;
    border-bottom: 1px solid rgb(230,230,232);
    color: rgb(144,147,153);
}
.personal-middle{
    height: 150px;
    border-bottom: 1px solid rgb(230,230,232);
    text-align: center;
}
.personal-middle table{
    margin: 0 auto;
    width:320px;
    height:150px;
    text-align: center;

}
.personal-footer{
    height: 50px;
    line-height: 50px;
    text-align: center;
    color: rgb(0,145,255);
}
</style>