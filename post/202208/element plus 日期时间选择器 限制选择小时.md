# element plus 日期时间选择器 限制选择小时

## el-date-picker 实现日期小时选择器

```js
<template>
    <div class="riqi">
        <el-date-picker
        v-model="currentTime"
        type="datetime"
        :default-value="defaultTime"
        format="YYYY-MM-DD HH:ss"
        value-format="YYYY-MM-DD HH"
        placeholder="选择时间"
        :disabled-date="disabledDate"
        :disabledHours="disabledHoursOption"
        :teleported="false"
        :clearable="false"
        @change="changeTime"
      />
    </div>
</template>
<script>
disabledHours() { 
    // 这个方法element plus 官方文档没有，只能查看github https://github.com/element-plus/element-plus/issues/162
    // 每次展开时间选择器下拉菜单，会触发一次这个方法
    // 当前小时之后的小时无法选择
    let hour = new Date().getHours()
    let disHours = []
    for(let i = hour; i < 24; i++) {
        disHours.push(i)
    }
    return disHours
    // 如果disHours为[] 就是都可以选
}

动态设置时间选择器
disabledHoursOption() {
    // 根据选择的日期，动态设置当天可以选择的小时范围
    if(日期时间选择器选择的时间 === "今天") {
        return [1,2,3]
    } else {
        return [4,5,6]
    }
}

</script>
<style lang="scss">
.riqi { // 隐藏 分秒容器，只显示小时容器
    position: absolute;
    z-index: 999;
    :deep(.el-time-spinner__wrapper) {
        width: 100%;
    }
    :deep(.el-scrollbar) {
        &:nth-of-type(2) {
            display: none;
        }
        &:nth-of-type(3) {
            display: none;
        }
    }
    :deep(.el-picker-panel__footer) {
        .is-text {
            display: none;
        }
    }
}
</style>
```