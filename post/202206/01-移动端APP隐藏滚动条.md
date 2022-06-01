[返回首页](https://miniplus.site)

# 移动端 APP 隐藏滚动条 但 保留滚动功能

有个 H5 页面要内嵌到 APP 中，根据需求，需要在 APP 里隐藏纵向滚动条[窗口右侧].

记录一下实现过程:

```css
html::-webkit-scrollbar,
body::-webkit-scrollbar {
  display: none;
}

html,
body {
  height: 100%;
  overflow: hidden;
  overflow-y: scroll;
  /* 使得ios滑动流畅 */
  -webkit-overflow-scrolling: touch;
}
```

以上代码可以实现隐藏纵向滚动条。

虽然可以实现隐藏滚动条并且保留滚动功能，但是会出现一个问题，就是用 Js 动态设置 scrollTop 会失效.

遇到的场景是，导航栏固定，列表容器滚动，然后切换导航，列表容器在上一步滚动后的位置不会复位，即使设置了 scrollTop = 0，也没用。

针对这个问题的解决方案：

```js
// 在每次切换导航切换的时候会执行一个方法，在方法内部添加如下代码
// 让scrollTop=0生效
let offset = document.documentElement || document.body;
document.querySelector("html").style.height = "auto";
document.querySelector("body").style.height = "auto";
offset.scrollTop = 0;
document.querySelector("html").style.height = "";
document.querySelector("body").style.height = "";
```

头部导航布局弃用 fixed，改用`postition: sticky`。

完。

另外：如果要隐藏的是某个容器内的滚动条，直接用

```css
.container::-webkit-scrollbar {
  display: none;
}
```
