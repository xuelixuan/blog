# 采用 mockjs 项目上传报错的解决方案

上传图片过程中报了
`upload.addEventListener is not a function`

网上查了相关解决方案,记录在下面:

根据网上的说法:

> 在 node_modules/mockjs/dist/mock.js  第 8308 行   和   node_modules/mockjs/src/xhr/xhr.js 第 216
> 添加代码： MockXMLHttpRequest.prototype.upload = xhr.upload;

按照上面的方法,添加后报 xhr 错误.

实际上应该添加在上述提示代码行附近.找到

`var xhr = createNativeXMLHttpRequest()`

这行代码,然后添加到后面即可.

```js
var xhr = createNativeXMLHttpRequest()
MockXMLHttpRequest.prototype.upload = xhr.upload
```

添加完成后,重启项目.
