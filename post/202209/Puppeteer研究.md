
## 什么是puppeteer？

- 一个Nodejs库，支持调用chrome的API来操纵WEB。
- 与Selenium 或是 PhantomJs相比，最大的优势是DOM是在v8引擎中操作，而不需要打开浏览器。

## puppeteer 能干什么？

- 利用网页生成PDF、图片
- 爬取SPA应用，并生成预渲染内容（即“SSR” 服务端渲染）
- 可以从网站抓取内容
- 自动化表单提交、UI测试、键盘输入等
- 帮你创建一个最新的自动化测试环境（chrome），可以直接在此运行测试用例6.捕获站点的时间线，以便追踪你的网站，帮助分析网站性能问题

## 安装puppeteer

安装puppeteer，会自动下载Chromium，大概在200m，但是Chromium下载地址国内经常被墙，因此可以更换为淘宝源

```
npm install puppeteer --registry=https://registry.npm.taobao.org --save
```

## 常用API

```javascript
const puppeteer = require('puppeteer');
const browser = puppeteer.launch({ //启动浏览器实例
    // 若是手动下载的chromium需要指定chromium地址, 默认引用地址为 /项目目录/node_modules/puppeteer/.local-chromium/
  executablePath: '/Users/huqiyang/Documents/project/z/chromium/Chromium.app/Contents/MacOS/Chromium',
    //设置超时时间
  timeout: 15000,
  //如果是访问https页面 此属性会忽略https错误
  ignoreHTTPSErrors: true,
  // 打开开发者工具, 当此值为true时, headless总为false
  devtools: false,
  // 关闭headless模式, 不会打开浏览器
  headless: false, // 有浏览器界面启动
  // --remote-debugging-port=3333会启一个端口，在浏览器中访问http://127.0.0.1:3333/可以查看
  args: ['--remote-debugging-port=3333', '--window-size=1280,960'],
  slowMo: 100, // 放慢浏览器执行速度，方便观察
});
// 链接一个已经存在的浏览器实例
let version = await request({
        uri:  "http://127.0.0.1:9222/json/version",
        json: true
    });
//直接连接已经存在的 Chrome
let browser = await puppeteer.connect({
    browserWSEndpoint: version.webSocketDebuggerUrl
});
// 这两种方式的对比：
// puppeteer.launch 每次都要重新启动一个 Chrome 进程，启动平均耗时 100 到 150 ms，性能欠佳
// puppeteer.connect 可以实现对于同一个 Chrome 实例的共用，减少启动关闭浏览器的时间消耗
// puppeteer.launch 启动时参数可以动态修改
// 通过 puppeteer.connect 我们可以远程连接一个 Chrome 实例，部署在不同的机器上
// puppeteer.connect 多个页面共用一个 chrome 实例，偶尔会出现 Page Crash 现象，需要进行并发控制，并定时重启 Chrome 实例
const page = browser.newPage(); // 创建一个新页面
page.goto('https://www.baidu.com'); // 进入指定网页
// 等待页面加载完成
page.goto(url, {
  waitUntil: 'networkidle2', // waitUntil参数是来确定满足什么条件才认为页面跳转完成 
  // load:页面load事件触发时；
  // domcontentloaded:页面的DOMContentLoaded事件触发时; 
  // networkidle0:不再有网络连接时触发（至少500毫秒后）; 
  // networkidle2:只有2个网络连接时触发（至少500毫秒后）;
})
// 等待元素、请求、响应
// page.waitForXPath：等待 xPath 对应的元素出现，返回对应的 ElementHandle 实例
// page.waitForSelector ：等待选择器对应的元素出现，返回对应的 ElementHandle 实例
// page.waitForResponse ：等待某个响应结束，返回 Response 实例
// page.waitForRequest：等待某个请求出现，返回 Request 实例
await page.waitForXPath('//img');
await page.waitForSelector('#uniqueId');
await page.waitForResponse('https://d.youdata.netease.com/api/dash/hello');
await page.waitForRequest('https://d.youdata.netease.com/api/dash/hello');
page.waitFor(2000); // 实在没办法了，只能用这个方法，页面等待可以是时间、某个元素、某个函数



page.goBack(); // 回退到上一个页面
page.goForword(); // 前进到下一个页面
page.reload(); // 重新加载页面
page.close(); // 关闭页面
const cookies = [{
name: 'token', 
value: 'system tokens',  //你的系统token
domain: 'domain' // 需要种在哪个域下
}]
page.setCookie(...cookies); //设置页面cookie
page.title(); // 获取页面标题
page.screenshot({
    path: 'jianshu.png',
    type: 'png',
    quality: 100, 只对jpg有效
    fullPage: true,
    // 指定区域截图，clip和fullPage两者只能设置一个
    // clip: {
    //  x: 0,
    //  y: 0,
    //  width: 1000,
    //  height: 40
    // }
}); // 网页截图
// 表单没有placeholder等预先的文字
page.type('.text.j-flag', '输入一段文字', {delay: 0}); // 点击输入框，并输入一段文字，模拟键盘输入
// 表单种已经存在文字，也可以通过另外一种方式模拟输入
async  inputTitle(article, editorSel, task) {
    const el = document.querySelector(editorSel.title)
    el.focus();// 获得焦点
    el.select(); // 选中所有文字
    document.execCommand('delete', false);// 删除文字
    document.execCommand('insertText', false, task.title || article.title); // 表单输入文字
}
const el = page.$(selector); // 获取页面元素
// el.click()
page.click('.el-form-item:nth-child(2) .el-form-item__content label:nth-child(1)')
page.keyboard.press('Enter'); // 按下回车，进行搜索
page.on('response', response => { // 监听页面是否有API响应
    const req = response.request()
    console.log(`Response的请求地址：${req.url()}，请求方式是：${req.method()}, 请求返回的状态${response.status()},`)
    let message = response.text()
    message.then(function (result) {
        console.log(`返回的数据：${result}`)
    })
})
const iframe = page.frames().find(f => f.name() === 'contentFrame'); //获取当前页面所有的 iframe，然后根据 iframe 的名字精确获取某个想要的 iframe
const SONG_LS_SELECTOR = await iframe.$('.btn'); //获取 iframe 中的某个元素
await SONG_LS_SELECTOR.click(); // 点击某个元素
 // 在浏览器中执行函数，相当于在控制台中执行某个函数，返回promise，批量采集逻辑一般都写到这里
 // 这就意味着，document.querySelector等前端公开接口都能使用
iframe.evaluate(e => {
  const songList = Array.from(e.childNodes);
  const idx = songList.findIndex(v => v.childNodes[1].innerText.replace(/\s/g, '') === '鬼才会想起');
  return songList[idx].childNodes[1].firstChild.firstChild.firstChild.href;
}, SONG_LS_SELECTOR);
iframe.$eval('.sub.s-fc3', e => e.innerText); //相当于在 iframe 中运行 document.queryselector 获取指定元素，并将其作为第一个参数传递
iframe.$$eval('.itm', elements => { // 相当于在 iframe 中运行 document.querySelectorAll 获取指定元素数组，并将其作为第一个参数传递
  const ctn = elements.map(v => {
   return v.innerText.replace(/\s/g, '');
  });
  return ctn;
 });
browser.close(); // 关闭浏览器
```

## 网上找到的部分资料
https://www.toutiao.com/article/6891511371465392654/
https://www.zhangshengrong.com/p/7B1LjQGVNw/


https://segmentfault.com/a/1190000023601892
https://bbs.huaweicloud.com/blogs/215632
https://cnodejs.org/topic/5a4d8d2299d207fa49f5cbbc
https://www.nhooo.com/note/qag83d.html
https://juejin.cn/post/6844903944645246984

https://codeantenna.com/a/TJpHsCnBoA
https://blog.51cto.com/u_15127592/3274603
https://juejin.cn/post/6844903545355894797?hmsr=joyk.com&utm_source=joyk.com&utm_medium=referral
https://juejin.cn/post/6844903565048168455
https://juejin.cn/post/6844904147527925767
https://juejin.cn/post/6882332163052994574
https://juejin.cn/post/6844903987947257870
https://juejin.cn/post/6844904206063648782
https://juejin.cn/post/6844903983144927240
https://juejin.cn/post/6957601771694850062
https://juejin.cn/post/6844903680626409486
https://www.shuzhiduo.com/A/QV5ZDwm6zy/
https://zhoushaw.github.io/2019/04/23/learning/node/puppeteer/
https://chowdera.com/2022/199/202207161109567279.html
https://github.com/Mrlhz/puppeteer
https://www.cxymm.net/article/ZHH_Love123/88567702
https://static.kancloud.cn/chandler/web_technology/2270683