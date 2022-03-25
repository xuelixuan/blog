#### box-shadow 复习

属性按顺序:

-   x 轴移动
-   y 轴移动
-   模糊距离
-   阴影尺寸
-   阴影颜色
-   外部阴影 or 内部引用

box-shadow: x y blur spread color inset

#### vue3 的 EventBus

安装`mitt`:`yarn add mitt`

新建 Bus.js 文件

```js
import mitt from "mitt";
export default mitt();

/**
 * import Bus from 'Bus.js'
 * 发布
 * Bus.emit('eventName', params)
 * 订阅
 * Bus.on('eventName', (params)=>{})
 * 取消订阅
 * Bus.off('eventName')
 */
```

#### 安装 React.js

安装 react cli :`npm install create-react-app -g`

#### vue 中动态获取元素的高

```js
1、html
<div ref="getheight"></div>
2、JavaScript
// 获取高度值 （内容高+padding+边框）
let height= this.$refs.getheight.offsetHeight;

// 获取元素样式值 （存在单位）
let height = window.getComputedStyle(this.$refs.getheight).height;

//获取元素内联样式值（非内联样式无法获取）
let height = this.$refs.getheight.style.height;


// 注意：要在元素渲染出来才能获取元素的高度
实例：
 mounted(){
    this.$nextTick(()=>{ // 页面渲染完成后的回调
        console.log(this.$refs.getheight.offsetHeight)
    })
}

来源:https://www.cnblogs.com/lymconch/p/11286971.html
```
