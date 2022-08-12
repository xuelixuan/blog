# echarts 数据节点位置添加自定义文字 markPoint

如果是按需引入echart，需要注意必须引入`MarkPointComponent`模块

工作中的应用场景是，在折线图的某个时间节点上要加上自定义文字

echarts配置

```js


// this.timeLabel
this.timeLabel.push(
    { 
        coord : [this.timeLine[index], this.timeData[index]],  // 自定义文字摆放的位置，与数据位置匹配
        value : key, // 这里写要展示在节点上的名字
        symbol: 'circle', // markPoint的背景形状
        symbolSize: [15, 15], // 背景大小
        label: {
            show: true, // 是否显示自定义文字
            position: 'top', // 自定义文字摆放的位置在该节点的相对位置
        }
    }
)


series: [
    {
        name: '热度值',
        type: 'line',
        stack: 'Total',
        markPoint: {
            data: this.timeLabel
        },
        data: this.timeData
    },
]
```