# element plus 根据路由信息自动输出面包屑导航



根据路由信息自动展示面包屑导航
```html
<template>
    <el-breadcrumb :separator-icon="ArrowRight">
        <el-breadcrumb-item to="/">首页</el-breadcrumb-item>
        <template v-for="item in $route.matched" :key="item">
            <template v-if="item.meta.isShow || item.name==='detail'">
                <el-breadcrumb-item :to="{ path: item.path }">{{item.meta.title}}</el-breadcrumb-item>
            </template>
        </template>
    </el-breadcrumb>
</template>
<script setup>
        import {
        ArrowRight
    } from '@element-plus/icons-vue'
</script>
<style lang="scss" scoped>
.el-breadcrumb {
    margin-bottom: 15px;
    :deep(.el-breadcrumb__item:not(:last-child)) {
        .el-breadcrumb__inner{
            color: #707070;
            a {
                &:hover {
                    color: rgb(64, 158, 255);
                }
            }
            &:hover {
                    color: rgb(64, 158, 255);
            }

        }
    }
}
</style>
```