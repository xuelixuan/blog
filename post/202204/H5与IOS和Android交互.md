## 判断当前设备

```js
// 原生APP端交互
var UA = navigator.userAgent
var isAndroid = UA.indexOf('Android') > -1 || UA.indexOf('Adr') > -1 //android终端
var isios = !!UA.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/) //ios终端
```

## 与原生平台交互

```js
let local = window.location.href.split('#')[0] + '#/' + this.shareUrl + '?uid=0'
let ua = navigator.userAgent.toLowerCase()
if (ua.match(/MicroMessenger/i) == 'micromessenger') {
	this.$emit('showOverlay')
} else {
	let openTime = new Date()
	if (isAndroid) {
		if (this.isApp) {
			// 调用安卓开发定义好的方法,注意方法名的位置
			window.app.goCommunity(url)
		} else {
			window.location =
				'自定义协议://www.xxx.com/openApp?page_url=-native-h5?url=' +
				local
			window.setTimeout(function () {
				window.location = 'https://www.xxx.com/download/xxx_1.1.8.apk'
			}, 2000)
		}
	} else if (isios) {
		if (this.isApp) {
			// 调用ios开发定义好的方法,注意方法名的位置
			window.webkit.messageHandlers.goCommunity.postMessage({
				url: url,
			})
		} else {
			window.setTimeout(function () {
				if (new Date() - openTime < 3100)
					window.location.href =
						'https://apps.apple.com/cn/app/id12345678'
			}, 3000)
			window.location = '自定义协议://-native-h5?url=' + local
		}
	}
}
```

附上两个链接:
https://www.jianshu.com/p/477ea20b1ece
https://www.cnblogs.com/dailc/p/8097598.html
