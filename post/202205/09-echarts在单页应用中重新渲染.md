[返回首页](https://miniplus.site)

# echarts 在单页应用中切换路由后不重新渲染

```js
echarts.init(document.getElementById("disChart")).dispose(); // 解决不重新渲染的问题
let disChart = echarts.init(document.getElementById("disChart"));
```
