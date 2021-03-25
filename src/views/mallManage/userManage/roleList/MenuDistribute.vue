<template>
    <el-container>
        <el-header height='120px'>
            <el-breadcrumb separator="/">
                <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
                <el-breadcrumb-item>用户管理</el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>角色列表</a></el-breadcrumb-item>
                <el-breadcrumb-item><a href='/'>分配菜单</a></el-breadcrumb-item>
            </el-breadcrumb>
            <br />
            <span style='font-size: 18px;font-weight: bold'>分配菜单</span>
        </el-header>
        <el-main>

        </el-main>
    </el-container>
</template>

<script>
export default {
    name: 'DistributeMenu'
};
</script>

<style scoped>

</style>