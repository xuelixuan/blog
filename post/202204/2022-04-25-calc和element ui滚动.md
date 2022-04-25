# calc 和 tinymce 工具条滚动过程中固定

calc 是 css 自动计算尺寸的方法.

```css
.container {
	height: calc(100% - 64px);
	width: calc(100% - 64px);
}
```

实现可视窗口内容超出滚动,必须每一级父元素都要设置一个固定高度,可以利用 calc 方法实现自适应

tinymce 工具条随着上下滚动,自动吸附到父元素顶部

```js
      scrollInfo(num) {
        let blankHeight = document.querySelector('.ed-title').clientHeight + 40; // 标题的高度
        let _stickyToolbar = document.querySelector('.tox-editor-header'); // tinymce 工具条
        let _reference = document.querySelector('.ed_position_reference'); // 标题和编辑器所在的父级元素
        if (num >= blankHeight) {
          _stickyToolbar.style.width = (_reference.clientWidth) + 'px'
          _stickyToolbar.style.position = 'fixed'
          _stickyToolbar.style.left = 'calc(100% - ' + (_reference.clientWidth + 20) + 'px)';
          _stickyToolbar.style.top = (_reference.offsetTop + 190) + 'px'
        } else {
          _stickyToolbar.style.position = 'static'
          _stickyToolbar.style.width = 'auto'
        }
      }
```

html 部分

```html
<div class="createStory ed_position_reference">
	<el-scrollbar style="height:100%;" ref="scrollbar"> 内容区 </el-scrollbar>
</div>
```
