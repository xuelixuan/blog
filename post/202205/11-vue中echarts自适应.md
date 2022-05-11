# vue 中 echarts 如何做到自适应?

第一步,单独封装 echarts 配置项

```js
// 获取echart配置
getOptions() {
    var data = [].concat([], this.list);
    var a = 0;
    for (var i = 0; i < data.length; i++) {
        a += data[i].value;
    }
    data.push({
        value: 3 * a,
        name: "",
        itemStyle: {
            normal: {
                color: "rgba(0,0,0,0)",
            },
        },
        tooltip: {
            show: false, // 是否显示提示框组件
        },
    }); //生成3倍大小的扇区
    return {
        tooltip: {
            trigger: "item",
            // formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
            // 图例
            orient: "horizontal",
            // x: "center",
            y: "bottom",
            right: this.right,
            textStyle: {
                fontSIZE: this.fontChart(1.2),
                color: "#ffffff", //字体颜色
            },
            data: this.legend,
        },
        series: [{
            name: "访问来源", // 鼠标移入每个扇区时显示的文本
            type: "pie",
            startAngle: 180, //从坐标系的哪个角度开始显示
            // radius: 220, //半径
            // center: [390, 250], //圆心坐标
            radius: this.radius,
            center: this.center, // 以哪个坐标为圆心开始绘制图形
            data: data,
            itemStyle: {
                emphasis: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: "rgba(0, 0, 0, 0.5)",
                },
            },
        }, ],
    };
},
```

第二步,定义自适应字体方法.

该方法默认按照 2560 设计稿进行屏幕分割.
将屏幕分按照 1rem=10px 分割成 256 分.
传参需要注意,如果想要定义一个 fontsize=16px,则传给该方法的实际数值是 1.6(rem)

```js
// 适配echart字体
fontChart(res) {
    //获取到屏幕的宽度
    var clientWidth =
        window.innerWidth ||
        document.documentElement.clientWidth ||
        document.body.clientWidth;
    if (!clientWidth) return; //报错拦截：
    let w = 0;
    let fontSize = 10 * (clientWidth / 2560);
    return res * fontSize;
},
```

第三步(可选): 根据浏览器尺寸显示不同尺寸的图

```js
// 根据浏览器尺寸显示不同尺寸的图
onDevice() {
    let width = window.innerWidth ||
        document.documentElement.clientWidth ||
        document.body.clientWidth;
    if (width > 1920 && width <= 2560) {
        this.radius = this.fontChart(22);
        this.center = [this.fontChart(39), this.fontChart(25)];
        this.right = "4%";
    }
    if (width <= 1920) {
        // console.log('1920');
        this.radius = this.fontChart(18);
        this.center = [this.fontChart(36.5), this.fontChart(20)];
        this.right = "1%"
    }
},
```

第四步,初始化图标

```js

initChart() {
    echarts.init(document.getElementById("disChart")).dispose(); // 解决不重新渲染的问题
    let disChart = echarts.init(document.getElementById("disChart"));

    let disOpt = this.getOptions(); // 动态获取配置
    disChart.setOption(disOpt);
    // resize
    window.addEventListener("resize", () => {
        this.onDevice();
        disChart.setOption(this.getOptions(), true); // 动态更新配置
        disChart.resize();
    });
},
```
