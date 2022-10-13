# puppeteer 安装与运行

先安装 cnpm
`npm install -g cnpm -registry=https://registry.npm.taobao.org`

再安装 puppeteer

`cnpm install puppeteer`

最后运行 demo

```js
const puppeteer = require('puppeteer')
;(async () => {
    const browser = await puppeteer.launch({ headless: false })
    const page = await browser.newPage()
    await page.goto('https://baidu.com')
    await page.screenshot({ path: 'example.png' })
    await browser.close()
})()
```
