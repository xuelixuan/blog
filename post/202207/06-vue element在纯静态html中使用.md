# vue element在纯静态html中使用

在html中引入文件

```html
<link href="https://lf26-cdn-tos.bytecdntp.com/cdn/expire-1-M/element-ui/2.15.7/theme-chalk/index.min.css"
<script src="https://lf26-cdn-tos.bytecdntp.com/cdn/expire-1-M/vue/2.6.14/vue.min.js"></script>
<script src="https://lf6-cdn-tos.bytecdntp.com/cdn/expire-1-M/element-ui/2.15.7/index.min.js"></script>
```

html中要标注出vue的作用范围
```
<div id="app"></div>
```

要想让element ui生效,除了在html当中引入element ui的标签外,一定要在js文件中实例化vue
```javascript
let vm = new Vue()
```