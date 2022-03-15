# 重新认识 position

## 所有属性值

-   position:static; (default)
-   position:relative;
-   position:absolute;
-   position:fixed;
-   position:sticky;
-   position: inherit

## 重点 sticky

-   与 relative，static 属性相同，sticky 布局下的元素不会脱离文档流

粘性布局。

```html
<div class="container">
    <div class="sticky-box">内容1</div>
    <div class="sticky-box">内容2</div>
    <div class="sticky-box">内容3</div>
    <div class="sticky-box">内容4</div>
</div>
```

```css
.container {
    background: #eee;
    width: 600px;
    height: 1000px;
    margin: 0 auto;
}

.sticky-box {
    position: -webkit-sticky;
    position: sticky;
    height: 20px;
    margin-bottom: 10px;
    background: #ff7300;
    top: 10px;
}

div {
    font-size: 12px;
    text-align: center;
    color: #fff;
    line-height: 20px;
}
```
