# echarts鼠标移入图标改变背景色

使用`emphasis`属性进行配置

```javascript
function initChart () {
    let dateList = JSON.parse(localStorage.getItem('echartsList'));
    option.series = [];
    let colorlist = JSON.parse(localStorage.getItem('colorlist'));
    dateList.forEach((item, index) => {
        console.log('item', item);
        let newDate =
        {
            name: item.name,
            type: 'line',
            // smooth: 0.5,
            // symbol: 'none',
            itemStyle: {
                normal: {
                    color: colorlist[index],
                    lineStyle: {
                        color: colorlist[index],
                        width: 1
                    }
                }
            },
            areaStyle: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{

                    offset: 0,
                    color: 'rgba(227, 233, 254,1)'

                }, {
                    offset: 1,
                    color: 'rgba(255, 255, 255,1)'
                }]),            //渐变色
                emphasis: {
                    color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{

                        offset: 0,
                        color: colorlist[index]

                    }, {
                        offset: 1,
                        color: 'rgba(255, 255, 255,1)'
                    }]),
                    focus: 'series', // 鼠标移入时,只显示当前,隐藏其他
                }
            },
            emphasis: {},
            smooth: true,
            data: item.value
        };
        option.series.push(newDate);
    });

    option.xAxis.data = JSON.parse(localStorage.getItem('timelist'));

    // 使用刚指定的配置项和数据显示图表。
    myChart.setOption(option);
    console.log(option);
}
```