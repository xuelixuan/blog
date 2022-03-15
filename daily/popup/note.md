#### 笔记

##### outline

元素有 border 属性，outline 设置元素 border 外围的样式

三个参数分别是

-   颜色
-   样式
-   宽度

```css
p {
    border: 1px solid red;
    outline: green dotted thick;
}
```

##### transform

旋转、缩放、倾斜或平移元素。

常用的三个属性：

-   平移
-   缩放
-   旋转

```css
transform: translate(x, y);
transform: scale(x, y);
transform: rotate(角度);
```

##### 动画效果

`transition`参数：

-   property 设定 css 属性名，可以是 width，height 或 transform
-   duration 完成过渡效果需要多少秒或毫秒
-   timing-function 速度曲线
-   delay 何时开始过渡

```css
transition: transform 0.4s, top 0.4s;
```

##### 水平垂直居中

利用`transition`实现居中布局，可以实现不定宽高元素的水平和垂直居中。

```css
div {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

##### 盒模型复习

`box-sizing: border-box` 将当前盒模型切换到 IE 怪异模型。

这样设置的好处是，宽度一旦写死，不论 padding+border 增加多少，宽度都不会增加。
