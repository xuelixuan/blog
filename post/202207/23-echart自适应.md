# echart 根据容器大小自适应

```javascript
window.addEventListener("resize", () => {
    console.log('resize', proxy.$refs.liteChart.offsetWidth, proxy.$refs.liteChart.offsetHeight);
    let oHeight = proxy.$refs.overview.offsetHeight;
    let lHeight = proxy.$refs.liteInfo.offsetHeight;
    modalChart.resize({
        width: proxy.$refs.liteChart.offsetWidth,
        height: oHeight - lHeight - 30,
    });
});
```