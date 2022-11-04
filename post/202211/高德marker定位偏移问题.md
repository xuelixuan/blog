# 高德 marker 定位偏移问题

拖拽后的状态和保存数据后重新渲染的状态不同，造成位置偏移。

造成偏移问题产生的原因，是因为鼠标点击 marker 时，获取到的是鼠标点击位置的经纬度，而不是 marker 所指的经纬度，所以造成了位置偏移。

解决方案如下：

```javascript
AMap.event.addListener(marker, 'dragging', function (e) {
    var lat = e.lnglat.lat,
        lng = e.lnglat.lng
    marker.setPosition(new AMap.LngLat(lng, lat))
})
```

参考：
https://www.cnblogs.com/hanshuai/p/14927181.html
https://www.shili8.cn/article/detail_20000036853.html
https://www.jianshu.com/p/cfcf8e3c9667
