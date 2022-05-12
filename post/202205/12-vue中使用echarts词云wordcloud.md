# vue 中使用 echarts 词云 wordcloud

安装 echart 和词云

```js
    "echarts": "^5.3.2",
    "echarts-wordcloud": "^2.0.0",
```

vue 代码

```js
<script>
    import * as echarts from "echarts/core";
    import "echarts-wordcloud";
    export default {
        data() {
            return {
                list: [],
                type: "all", // car person
            };
        },
        mounted() {
            this.getList();
        },
        methods: {
            initWordCloud() {
                echarts.init(document.getElementById("wordCloudChart")).dispose(); // 解决不重新渲染的问题
                // console.log('list', this.list)
                let wordChart = echarts.init(
                    document.getElementById("wordCloudChart")
                );

                let maskImage = new Image();
                maskImage.setAttribute('crossOrigin', '');
                maskImage.src = require("../../assets/img/car.jpg"); // 词云会按照图片的形状来渲染
                // 解决报错IndexSizeError: Failed to execute 'getImageData' on 'CanvasRenderingContext2D': The source width is 0.
                maskImage.width = maskImage.width || maskImage.naturalWidth
                maskImage.height = maskImage.height || maskImage.naturalHeight
                let _this = this;
                maskImage.onload = function () {
                    const wordOpt = {
                        series: [{
                            type: "wordCloud",
                            left: "center",
                            top: "center",
                            width: "100%",
                            height: "100%",
                            maskImage: maskImage,
                            gridSize: 0, //值越大，word间的距离越大，单位像素
                            sizeRange: [4, 36], //word的字体大小区间，单位像素
                            rotationRange: [-90, 90], //word的可旋转角度区间
                            textStyle: {
                                color: function () {
                                    // Random color
                                    return (
                                        "rgb(" + [
                                            Math.round(Math.random() * 255),
                                            Math.round(Math.random() * 255),
                                            Math.round(Math.random() * 255),
                                        ].join(",") +
                                        ")"
                                    );
                                },
                                emphasis: {
                                    shadowBlur: 2,
                                    shadowColor: "#000",
                                },
                            },
                            data: _this.list,
                        }, ],
                        backgroundColor: "rgba(0,0,0,0)",
                    };
                    wordChart.setOption(wordOpt);
                };
                // resize
                window.addEventListener("resize", throttle(wordChart.resize, 200));
            },
            getList() {
                getWordCloudData(this.type).then((res) => {
                    this.list = res.data.list;
                    this.initWordCloud();
                });
            },
        },
    };
</script>
```
