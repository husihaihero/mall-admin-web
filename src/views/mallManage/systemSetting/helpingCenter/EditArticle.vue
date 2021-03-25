<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>系统设置</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>帮助中心</a></el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>编辑</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>编辑</span>
            <br />
            <span style='float: right'>
                <el-button  size='1'>返回</el-button>
                <el-button @click='addResourceDrawer = true' type='primary' size='1'>保存</el-button>
            </span>
        </el-header>
        <el-main >
            <div class='article-info'>
               <p style='font-size: 18px;font-weight: bold'>基本信息</p>
                <el-divider></el-divider>
                <el-input v-model="title" max='100' show-word-limit>文章的标题</el-input>
                <div style="margin: 20px 0;"></div>
                <el-input
                    type="textarea"
                    placeholder="请输入内容"
                    v-model="textarea"
                    maxlength="120"
                    show-word-limit
                    :rows="5"
                >
                </el-input>
                <div style='margin-top:15px;'>
                    <span style='margin-right: 15px'>分类</span>
                    <el-select>

                    </el-select>
                    <span style='float: right'>是否启用：
                        <el-switch
                            v-model="status1"
                            active-color="#13ce66"
                            inactive-color="#ff4949">
                         </el-switch>
                    </span>
                </div>

            </div>
            <div class='article-cover'>
                <p style='font-size: 18px;font-weight: bold;margin-bottom: 5px'>文章封面</p>
                <p style='color: #8c939d'>支持JPG/PNG格式</p>
                <el-divider></el-divider>
                <div style="margin: 20px 0;"></div>
                <div>
                    <el-image :src="articlePicture"></el-image>
                </div>
                <div style='text-align: center'><span>更改图片</span></div>
            </div>
            <div class='article-content'>
                <p style='font-size: 18px;font-weight: bold'>文章内容</p>
                <el-divider></el-divider>
                <div style="margin: 20px 0;"></div>
                <el-input
                    type="textarea"
                    placeholder="请输入内容"
                    v-model="textContent"
                    maxlength="120"
                    show-word-limit
                    :rows="15"
                >
                </el-input>
            </div>

        </el-main>

    </el-container>
</template>

<script>
export default {
    name: 'EditArticle',
    data(){
        return{
            title:"文章标题",       //文章标题
            textarea:"文章简洁简介", //文章简介
            status1:true,      //是否启用
            textContent:"内容内容内容内容内容内容",      //是否内容
            articlePicture:"https://fuss10.elemecdn.com/3/28/bbf893f792f03a54408b3b7a7ebf0jpeg.jpeg",
            value:""
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
    padding:0;
    font-size: 14px;
}
.article-info{
    background-color:white;
    float:left;
    width: 71%;
    height: 30%;
    padding: 1%;
    margin-bottom: 20px;
}
.article-cover{
    background-color: white;
    float: right;
    width: 24%;
    padding: 1%;
}
.article-content{
    background-color: white;
    float:left;
    width:71%;
    height:30%;
    padding: 1%;
}
</style>