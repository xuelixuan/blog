# echarts 地图实现的一些功能

## 中国地图

-   单击地图显示相关地区资讯

```js
chinaMap.on("click", (params) => {
	// console.log('省份图：',params.name + '的数据', params.data)
	if (params.data !== undefined) {
		this.$store.commit("home/setProvinceList", params.data.children);
	} else {
		this.$store.commit("home/setProvinceList", []);
	}
	this.$store.commit("home/setProvinceTitle", params.name);
	chinaMap.setOption({
		title: {
			text: `${this.todayStr} ${this.hour}时 ${params.name}数据`,
		},
	});
});
```

-   双击地图进入某省份地图
    思路中国地图和省份地图分别渲染，做成两个组件，来回切换

```js
// 双击地图进入省份地图
chinaMap.on("dblclick", (params) => {
	this.$emit("show", true);
});
```

-   放大地图，省份名称下显示自定义数字信息

```js
// 放大地图后显示省份名称
chinaMap.on("georoam", () => {
	if (chinaChart) {
		let { zoom, regions } = chinaChart.getOption().geo[0];
		if (zoom > 4) {
			chinaChart.setOption({
				geo: {
					label: {
						show: true,
						formatter: (params) => {
							let countName = "";
							this.list.forEach((item) => {
								if (item.name === params.name) {
									countName = this.formatData(item.value);
								}
							});
							return `${params.name}\n${countName || "0"}`;
						},
					},
				},
			});
		} else {
			chinaChart.setOption({
				geo: {
					label: {
						formatter: (params) => {
							return params.name;
						},
					},
				},
			});
		}
	}
});
```

-   鼠标移入 li 标签，联动地图选中某省份
    实现思路，li 鼠标移入，给 store 中的变量赋值
    地图所在组件通过监听 store 中的变量，动态给地图设置选中状态

```js
watch: {
    "$store.state.home.hoverProvince": function (hoverProvince) {
        console.log({ hoverProvince });
        // if (Object.keys(hoverProvince).length === 0) return;
        // 手动设置省份选中状态
        let temp = chinaChart.getOption();
        chinaChart.clear();
        // chinaChart.off("click");
        console.log({ temp });
        temp.series[0].data.forEach((item) => {
            item.selected = false;
        });
        if (hoverProvince.name) {
            temp.series[0].data.forEach((item) => {
                if (item.name == hoverProvince.name) {
                    item.selected = true;
                    temp.series[0].selectedMap = {
                        [item.name]: true,
                    };
                }
            });
        } else {
            temp.series[0].selectedMap = {};
        }
        chinaChart.setOption(temp);
    },
}
```

-   点击地图空白处，重置数据

```js
// 如果点击空白处，返回到全国数据
chinaMap.getZr().on("click", (params) => {
	if (!params.target) {
		console.log("点击了中国地图空白处");
		// 清空地图选中状态
		let temp = chinaChart.getOption();
		chinaChart.clear();
		temp.series[0].data.forEach((item) => {
			item.selected = false;
		});
		// temp.title.text = temp.title[0].text = `${this.todayStr} ${this.hour}时 ${this.province.name}数据`
		temp.series[0].selectedMap = {};
		chinaChart.setOption(temp);
		this.$store.commit("home/setProvinceMap", this.list);
		this.$store.commit("home/setProvinceList", this.list);
		//返回到中国地图初始化数据
		this.$store.commit("home/setProvinceTitle", "全国");
		chinaMap.setOption({
			title: {
				text: `${todayStr} ${hour}时 全国数据`,
			},
		});
	}
});
```

-   更改地图配色
    echarts 配置

```js
visualMap: {
    inRange: {
        color: ["#8BCEFF", "#008DBB"],
    },
}
```

-   地图显示筛选 bar

```js
visualMap: {
    show: true,
    min: 0,
    max: this.maxLen,
    left: "34px",
    bottom: "25px",
    itemHeight: 100,
    text: ["高", "低"],
    realtime: false,
    calculable: true,
}
```
