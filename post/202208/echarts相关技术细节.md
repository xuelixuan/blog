# echarts相关技术细节

## tooltip 添加图片

```js
// 悬浮窗
tooltip: {
    trigger: 'item',
    formatter: function (params) {
        if(params.value > 10000  && params.value < 100000000) {
            params.value = (params.value / 10000).toFixed(1) + '万';
        } else if(params.value >= 100000000) {
            params.value = (params.value / 100000000).toFixed(1) + '亿';
        }
        let url = require('@/assets/img/fire.svg')
        return `${params.name}: ${params.value || '0'} <img src="${url}">`
    }
},
```

## 渲染某省轮廓图

直接用中国地图中的省份数据当作省份地图渲染，这样省份就只显示轮廓。
不要用具体某省的数据，会显示该省下面的所有地级市

## 鼠标移入自定义标签，地图联动hover状态


```html
<li @mouseenter="enter(item)"></li>

<script>
    enter(item) {
        // 根据鼠标移入的相应item，做到匹配地图上的某个省份数据，然后高亮选中

        let temp = chinaChart.getOption();// 先把上一次的配置项保存在一个临时变量当中
        chinaChart.clear(); // 清空当前实例化对象所有配置项
        temp.series[0].data.forEach(item=>{
            item.selected = false  // series-data里的数据添加selected属性，echarts会根据这个属性判断是否选中
        })
        if(hoverProvince.name) {
            temp.series[0].data.forEach(item=>{
                if(item.name == hoverProvince.name) {
                    item.selected = true
                    temp.series[0].selectedMap = {
                        [item.name]: true
                    }
                }
            })
        } else {
            temp.series[0].selectedMap = {} // 只设置selected 为false不行，必须手动把这个属性置空
        }
        chinaChart.setOption(temp); // 重新加载配置项，实现鼠标联动
    }
</script>
```


## 鼠标点击echarts画布空白处，实现从地级返回市级

`.getZr().on`是默认写法

```js
 chinaMap.getZr().on('click', (params) => {
    if(!params.target) { // 如果点击空白处，执行
        // 清空地图选中状态
        let temp = chinaChart.getOption()
        chinaChart.clear()
        temp.series[0].data.forEach(item=>{
            item.selected = false
        })
        temp.title.text = temp.title[0].text = `${this.todayStr} ${this.hour}时 ${this.province.name}数据`; // 动态更改图标的标题，必须更改两处
        temp.series[0].selectedMap = {}
        chinaChart.setOption(temp);
    }
})
```