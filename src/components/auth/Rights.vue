<template>
    <div>
        <!-- 面包屑导航 -->
        <el-breadcrumb separator=">">
            <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>权限管理</el-breadcrumb-item>
            <el-breadcrumb-item>权限列表</el-breadcrumb-item>
        </el-breadcrumb>

        <el-card class="box-card">
            <el-table :data="rightsList" border stripe>
                <el-table-column type="index"></el-table-column>
                <el-table-column label="权限名称" prop="authName"></el-table-column>
                <el-table-column label="路径" prop="path"></el-table-column>
                <el-table-column label="权限等级" prop="level">
                    <template slot-scope="scope">
                        <el-tag v-if="scope.row.level == '0'">一级</el-tag>
                        <el-tag type="success" v-else-if="scope.row.level == '1'">二级</el-tag>
                        <el-tag type="warning" v-else>三级</el-tag>
                    </template>
                </el-table-column>
            </el-table>
        </el-card>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                rightsList: []
            }
        },
        created () {
            this.getRightsList();
        },
        methods: {
            async getRightsList() {
                const {data : res} = await this.$http.get('rights/list');
                if(res.meta.status !== 200) return this.$message.error("获取权限列表失败！")
                this.rightsList = res.data;
            },
            getLevelType(level) {
                switch(level){
                    case '0':
                        return "一级"
                    case '1':
                        return "二级"
                    case '2':
                        return "三级"
                }
            },
            getLevelColor(level) {
                switch(level){
                    case '0':
                        return "warning"
                    case '1':
                        return "success"
                    case '2':
                        return "info"
                }                
            }
        },
    }
</script>

<style lang="less" scoped>

</style>