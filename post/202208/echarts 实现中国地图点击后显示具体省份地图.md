# echarts 实现中国地图点击后显示具体省份地图



中国地图部分
```html
<template>
    <div class="china ln6" ref="china">
        <div v-if="!showProvince" class="chinaChart" id="chinaChart"  ref="chinaChart"></div>
        <provinceMap v-else :province="province" @show="showChinaChart"></provinceMap>
    </div>
</template>
<script>
    import echarts from '@/lib/chart-china.js'
    import chinaData from '@/utils/china.json';
    import provinceMap from './province-map.vue'
    import {
        getChinaData
    } from '@/http/home/api.js'
    export default {
        name: 'China',
        components: {
            provinceMap
        },
        data() {
            return {
                province: 'beijing',
                list: [],
                type: 'hot', // topic brand person
                showProvince: false,
                cMap: {
                    "北京": 'beijing',
                    "天津": 'tianjin',
                    "上海": 'shanghai',
                    "重庆": 'chongqing',
                    "黑龙江": 'heilongjiang',
                    "吉林": 'jilin',
                    "辽宁": 'liaoning',
                    "内蒙古": 'neimenggu',
                    "河北": 'hebei',
                    "山西": 'shanxi',
                    "陕西": 'shangxi',
                    "甘肃": 'gansu',
                    "青海": 'qinghai',
                    "宁夏": 'ningxia',
                    "新疆": 'xinjiang',
                    "西藏": 'xizang',
                    "四川": 'sichuan',
                    "云南": 'yunnan',
                    "贵州": 'guizhou',
                    "湖南": 'hunan',
                    "湖北": 'hubei',
                    "江西": 'jiangxi',
                    "江苏": 'jiangsu',
                    "浙江": 'zhejiang',
                    "安徽": 'anhui',
                    "福建": 'fujian',
                    "广东": 'guangdong',
                    "广西": 'guangxi',
                    "海南": 'hainan',
                    "山东": 'shandong',
                    "河南": 'henan',
                }
            }
        },
        mounted() {
            this.getList()
        },
        methods: {
            showChinaChart() {
                this.showProvince = false
                this.getList()
            },
            changeType(type) {
                this.type = type
                this.getList()
            },
            getList() {
                getChinaData(this.type).then(res => {
                    this.list = res.data.list
                    this.initChart()
                })
            },
            initChart() {
                echarts.init(document.getElementById("chinaChart")).dispose(); // 解决不重新渲染的问题
                echarts.registerMap('china', chinaData)
                let chinaMap = echarts.init(document.getElementById('chinaChart'))
                let chinaOpt = {
                    // 悬浮窗
                    tooltip: {
                        trigger: 'item',
                        formatter: function (params) {
                            return `${params.name}: ${params.value || '-'}`
                        }
                    },
                    // 图例
                    visualMap: {
                        show: false,
                        min: 0,
                        max: 100,
                        text: ['高', '低'],
                        realtime: false,
                        calculable: true,
                        inRange: {
                            color: ['rgba(0, 181, 242, 1)', 'yellow', 'orangered']
                        }
                    },
                    geo: {
                        map: "china",
                        roam: false, // 一定要关闭拖拽
                        zoom: 1.23,
                        center: [105, 36], // 调整地图位置
                        label: {
                            normal: {
                                show: false, //关闭省份名展示
                                fontSize: "10",
                                color: "rgba(0,0,0,0.7)"
                            },
                            emphasis: {
                                show: false
                            }
                        },
                        itemStyle: {
                            normal: {
                                areaColor: "#0d0059",
                                borderColor: "#389dff",
                                borderWidth: 1, //设置外层边框
                                shadowBlur: 0,
                                shadowOffsetY: 8,
                                shadowOffsetX: 0,
                                shadowColor: "#2a6eaf"
                            },
                            emphasis: {
                                areaColor: "#184cff",
                                shadowOffsetX: 0,
                                shadowOffsetY: 0,
                                shadowBlur: 5,
                                borderWidth: 0,
                                shadowColor: "rgba(0, 0, 0, 0.5)"
                            }
                        }
                    },
                    series: [{
                        type: "map",
                        map: "china",
                        roam: false,
                        zoom: 1.23,
                        center: [105, 36],
                        // geoIndex: 1,
                        // aspectScale: 0.75, //长宽比
                        showLegendSymbol: false, // 存在legend时显示
                        label: {
                            normal: {
                                show: false
                            },
                            emphasis: {
                                show: false,
                                textStyle: {
                                    color: "#fff"
                                }
                            }
                        },
                        itemStyle: {
                            normal: {
                                areaColor: "rgba(0, 181, 242, 1)",
                                borderColor: "#FFFFFF",
                                borderWidth: 0.5
                            },
                            emphasis: {
                                areaColor: "rgba(255, 178, 0, 1)",
                                shadowOffsetX: 0,
                                shadowOffsetY: 0,
                                shadowBlur: 5,
                                borderWidth: 0,
                                shadowColor: "rgba(0, 0, 0, 0.5)"
                            }
                        },
                        data: this.list
                    }]
                }
                // chinaMap.setOption(chinaOpt)
                chinaMap.setOption(chinaOpt)
                chinaMap.on('click', (params) =>{
                    this.showProvince = true
                    if(params.name === '台湾') return
                    this.province = this.cMap[params.name]
                })
                window.addEventListener("resize", () => {
                    console.log(this.$refs)
                    let oHeight = this.$refs.china.offsetHeight;
                    chinaMap.resize({
                        width: this.$refs.china.offsetWidth,
                        height: oHeight - 30,
                    });
                })
            }
        }
    }
</script>
```

- 通过点击中国地图，拿到当前点击省份的名称
- 通过汉字转拼音，直接拿到对应的省份拼音.json地图文件


省份地图渲染部分
```html
<!-- 省份地图 -->
<template>
    <div class="china ln6" ref="province">
        <a href="javascript:;" @click="$emit('show')">
            <el-icon><ArrowLeft /></el-icon>返回
        </a>
        <div class="chinaChart" :id="echartId"></div>
    </div>
</template>
<script>
    import { ArrowLeft } from '@element-plus/icons-vue'
    import echarts from '@/lib/chart-china.js'
    import {
        getChinaData
    } from '@/http/home/api.js'
    export default {
        name: 'China',
        components: {
            ArrowLeft
        },
        props: {
            province: {
                type: String,
                default: 'beijing'
            }
        },
        data() {
            return {
                echartId: 'echarts_' + new Date().getTime() + Math.floor(Math.random() * 1000),
                list: [],
                type: 'hot', // topic brand person
            }
        },
        mounted() {
            // this.getList()
            this.initChart()
        },
        methods: {
            changeType(type) {
                this.type = type
                this.getList()
            },
            getList() {
                getChinaData(this.type).then(res => {
                    this.list = res.data.list
                    this.initChart()
                })
            },
            initChart() {
                // echarts.init(document.getElementById(this.echartId)).dispose(); // 解决不重新渲染的问题
                let chinaOpt = {
                    // 悬浮窗
                    tooltip: {
                        trigger: 'item',
                        formatter: function (params) {
                            return `${params.name}: ${params.value || '-'}`
                        }
                    },
                    // 图例
                    visualMap: {
                        show: false,
                        min: 0,
                        max: 100,
                        text: ['高', '低'],
                        realtime: false,
                        calculable: true,
                        inRange: {
                            color: ['rgba(0, 181, 242, 1)', 'yellow', 'orangered']
                        }
                    },
                    geo: {
                        map: '',
                        roam: false, // 不开启缩放和平移
                        zoom: 1.2, // 视角缩放比例
                        label: {
                            normal: {
                            show: true,
                            fontSize: 10,
                            color: '#000'
                            },
                            emphasis: {
                            show: true,
                            color: 'blue',
                            }
                        },
                        itemStyle: {
                            normal: {
                            borderColor: 'rgba(0, 0, 0, 0.2)'
                            },
                            emphasis: {
                            areaColor: 'skyblue', // 鼠标选择区域颜色
                            shadowOffsetX: 0,
                            shadowOffsetY: 0,
                            shadowBlur: 20,
                            borderWidth: 0,
                            shadowColor: 'rgba(0, 0, 0, 0.5)'
                            }
                        },

                    },
                    series: [
                        {
                        type: "map",
                        map: "beijing",
                        geoIndex: 0,
                        data: []
                    }]
                }
                // chinaMap.setOption(chinaOpt)
                chinaOpt.geo.map = 'beijing'
                let chinaMap = echarts.init(document.getElementById(this.echartId))
                echarts.registerMap('beijing', require(`@/utils/map/${this.province}.json`))
                chinaMap.setOption(chinaOpt)
                chinaMap.on('click', function (params) {
                    console.log({params}, params.name)
                })
                window.addEventListener("resize", () => {
                    let oHeight = this.$refs.province.offsetHeight;
                    chinaMap.resize({
                        width: this.$refs.province.offsetWidth,
                        height: oHeight - 30,
                    });
                })
            }
        }
    }
</script>
```