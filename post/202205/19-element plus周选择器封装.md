# element plus 周选择器封装

首先在 app.vue 中进行配置，将每周的开始日期设置为星期一.

```html
<template>
    <el-config-provider :locale="locale">
        <router-view></router-view>
    </el-config-provider>
</template>
<script>
    import { ElConfigProvider } from "element-plus";
    import zhCn from "element-plus/lib/locale/lang/zh-cn";
    import "dayjs/locale/zh-cn"; // 这一行是关键，可以将周选择器从周日开始改成从周一开始

    export default {
        components: {
            ElConfigProvider,
        },
        setup() {
            return {
                locale: zhCn,
            };
        },
    };
</script>
<style>
    #app {
        height: 100%;
    }
</style>
```

然后开始封装周选择器

```html
// oWeekPicker
<template>
    <el-date-picker
        v-model="weekRange"
        type="week"
        @change="changeTime"
        placeholder="选择周"
        value-format="YYYY-MM-DD"
        :format="startTimeStamp + ' 至 ' + endTimeStamp"
    />
</template>
<script>
    import { ref } from "vue";
    export default {
        props: {
            weekRange: {
                type: String,
                default: "",
            },
        },
        setup() {
            const startTimeStamp = ref(null);
            const endTimeStamp = ref(null);
            return {
                startTimeStamp,
                endTimeStamp,
            };
        },
        mounted() {
            if (this.weekRange) {
                // 编辑状态
                this.startTimeStamp = this.weekRange;
                this.endTimeStamp = this.dayPlus(this.weekRange, 6);
            }
        },
        methods: {
            changeTime(val) {
                if (val) {
                    // let timeStamp = val.getTime(); //标准时间转为时间戳，毫秒级别
                    // this.startTimeStamp = this.timeFun(timeStamp); //开始时间
                    // this.endTimeStamp = this.timeFun(timeStamp + 24 * 60 * 60 * 1000 * 6); //结束时间
                    this.startTimeStamp = val;
                    this.endTimeStamp = this.dayPlus(val, 6);
                    return this.$emit("change", {
                        startTimeStamp: this.startTimeStamp,
                        endTimeStamp: this.endTimeStamp,
                    });
                } else {
                    this.$emit("changeTime", {
                        startTimeStamp: null,
                        endTimeStamp: null,
                    });
                }
            },
            // 日期字符串增加6天
            dayPlus(dayStr, dayNum) {
                let timeStamp = new Date(dayStr).getTime(); //标准时间转为时间戳，毫秒级别
                timeStamp = timeStamp + 24 * 60 * 60 * 1000 * dayNum; //增加6天
                return this.timeFun(timeStamp); //时间戳转为日期字符串
            },
            // 时间戳转为日期字符串
            timeFun(unixtimestamp) {
                var unixtimestamp = new Date(unixtimestamp);
                var year = 1900 + unixtimestamp.getYear();
                var month = "0" + (unixtimestamp.getMonth() + 1);
                var date = "0" + unixtimestamp.getDate();
                return (
                    year +
                    "-" +
                    month.substring(month.length - 2, month.length) +
                    "-" +
                    date.substring(date.length - 2, date.length)
                );
            },
        },
    };
</script>
```

使用方法

```html
---新增 <oWeekPicker @changeTime="getChangeTime" />

--- 编辑
<oWeekPicker @changeTime="getChangeTime" :weekRange="weekRange" />

weekRange = '2022-05-16'; (这个时间是每周的星期一)
```
