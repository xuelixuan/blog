# background-clip

规定元素的背景绘制的范围.

当使用`background-image`对元素背景进行绘制的时候,可以使用`background-clip`对绘制的背景范围进行限制.

限制范围有四种:

1. border-box; 绘制的背景包括边框
2. padding-box; 绘制的背景范围不包括边框
3. content-box; 绘制的背景范围值包含元素本身
4. text; 绘制的背景范围只包含文字本身,因为文字本身有颜色,所以需要使用`color: transparent;`将当前文字颜色设置为透明,暴露出后面的背景颜色

文字渐变功能:

```css
.title {
	font-size: 42px;
	font-weight: bold;
	background-image: linear-gradient(to right, #03d7fd, #03f8fd, #009cd1);
	background-clip: text;
	color: transparent;
}
```
