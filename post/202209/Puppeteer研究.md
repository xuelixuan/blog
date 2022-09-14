
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
        // 优化chrome内存
        //   '--disable-gpu',
        // '--disable-dev-shm-usage',
        // '--disable-web-security',
        // '--disable-xss-auditor',    // 关闭 XSS Auditor
        // '--no-zygote',
        // '--no-sandbox',
        // '--disable-setuid-sandbox',
        // '--allow-running-insecure-content',     // 允许不安全内容
        // '--disable-webgl',
        // '--disable-popup-blocking',
  slowMo: 100, // 放慢浏览器执行速度，方便观察
  defaultViewport: {
        width: 1920,
        height: 1080
    },
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
// 自定义等待
// page.waitForFunction：等待在页面中自定义函数的执行结果，返回 JsHandle 实例
// page.waitFor：设置等待时间，实在没办法的做法
await page.goto(url, { 
    timeout: 120000, 
    waitUntil: 'networkidle2' 
});
//我们可以在页面中定义自己认为加载完的事件，在合适的时间点我们将该事件设置为 true
//以下是我们项目在触发截图时的判断逻辑，如果 renderdone 出现且为 true 那么就截图，如果是 Object，说明页面加载出错了，我们可以捕获该异常进行提示
let renderdoneHandle = await page.waitForFunction('window.renderdone', {
    polling: 120
});
const renderdone = await renderdoneHandle.jsonValue();
if (typeof renderdone === 'object') {
    console.log(`加载页面失败：报表${renderdone.componentId}出错 -- ${renderdone.message}`);
}else{
    console.log('页面加载成功');
}


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

### Case1：截图

```js

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    //设置可视区域大小
    await page.setViewport({width: 1920, height: 800});
    await page.goto('https://youdata.163.com');
    //对整个页面截图
    await page.screenshot({
        path: './files/capture.png',  //图片保存路径
        type: 'png',
        fullPage: true //边滚动边截图
        // clip: {x: 0, y: 0, width: 1920, height: 800}
    });
    //对页面某个元素截图
    let [element] = await page.$x('/html/body/section[4]/div/div[2]');
    await element.screenshot({
        path: './files/element.png'
    });
    await page.close();
    await browser.close();
})();
```
我们怎么去获取页面中的某个元素呢？

- page.$('#uniqueId')：获取某个选择器对应的第一个元素
- page.$$('div')：获取某个选择器对应的所有元素
- page.$x('//img')：获取某个 xPath 对应的所有元素
- page.waitForXPath('//img')：等待某个 xPath 对应的元素出现
- page.waitForSelector('#uniqueId')：等待某个选择器对应的元素出现

### case2: 模拟用户登录

```javascript
(async () => {
    const browser = await puppeteer.launch({
        slowMo: 100,    //放慢速度
        headless: false,
        defaultViewport: {width: 1440, height: 780},
        ignoreHTTPSErrors: false, //忽略 https 报错
        args: ['--start-fullscreen'] //全屏打开页面
    });
    const page = await browser.newPage();
    await page.goto('https://demo.youdata.com');
    //输入账号密码
    const uniqueIdElement = await page.$('#uniqueId');
    await uniqueIdElement.type('admin@admin.com', {delay: 20});
    const passwordElement = await page.$('#password', {delay: 20});
    await passwordElement.type('123456');
    //点击确定按钮进行登录
    let okButtonElement = await page.$('#btn-ok');
    //等待页面跳转完成，一般点击某个按钮需要跳转时，都需要等待 page.waitForNavigation() 执行完毕才表示跳转成功
    await Promise.all([
        okButtonElement.click(),
        page.waitForNavigation()  
    ]);
    console.log('admin 登录成功');
    await page.close();
    await browser.close();
})();
```
那么 ElementHandle 都提供了哪些操作元素的函数呢？

- elementHandle.click()：点击某个元素
- elementHandle.tap()：模拟手指触摸点击
- elementHandle.focus()：聚焦到某个元素
- elementHandle.hover()：鼠标 hover 到某个元素上
- elementHandle.type('hello')：在输入框输入文本


### case3：请求拦截
请求在有些场景下很有必要，拦截一下没必要的请求提高性能，我们可以在监听 Page 的 request 事件，并进行请求拦截，前提是要开启请求拦截 page.setRequestInterception(true)
```javascript
(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    const blockTypes = new Set(['image', 'media', 'font']);
    await page.setRequestInterception(true); //开启请求拦截
    page.on('request', request => {
        const type = request.resourceType();
        const shouldBlock = blockTypes.has(type);
        if(shouldBlock){
            //直接阻止请求
            return request.abort();
        }else{
            //对请求重写
            return request.continue({
                //可以对 url，method，postData，headers 进行覆盖
                headers: Object.assign({}, request.headers(), {
                    'puppeteer-test': 'true'
                })
            });
        }
    });
    await page.goto('https://demo.youdata.com');
    await page.close();
    await browser.close();
})();
```
那 page 页面上都提供了哪些事件呢？

- page.on('close') 页面关闭
- page.on('console') console API 被调用
- page.on('error') 页面出错
- page.on('load') 页面加载完
- page.on('request') 收到请求
- page.on('requestfailed') 请求失败
- page.on('requestfinished') 请求成功
- page.on('response') 收到响应
- page.on('workercreated') 创建 webWorker
- page.on('workerdestroyed') 销毁 webWorker


### case4：获取 WebSocket 响应
Puppeteer 目前没有提供原生的用于处理 WebSocket 的 API 接口，但是我们可以通过更底层的 Chrome DevTool Protocol (CDP) 协议获得
```javascript
(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    //创建 CDP 会话
    let cdpSession = await page.target().createCDPSession();
    //开启网络调试,监听 Chrome DevTools Protocol 中 Network 相关事件
    await cdpSession.send('Network.enable');
    //监听 webSocketFrameReceived 事件，获取对应的数据
    cdpSession.on('Network.webSocketFrameReceived', frame => {
        let payloadData = frame.response.payloadData;
        if(payloadData.includes('push:query')){
            //解析payloadData，拿到服务端推送的数据
            let res = JSON.parse(payloadData.match(/\{.*\}/)[0]);
            if(res.code !== 200){
                console.log(`调用websocket接口出错:code=${res.code},message=${res.message}`);
            }else{
                console.log('获取到websocket接口数据：', res.result);
            }
        }
    });
    await page.goto('https://netease.youdata.163.com/dash/142161/reportExport?pid=700209493');
    await page.waitForFunction('window.renderdone', {polling: 20});
    await page.close();
    await browser.close();
})();
```
### case5：植入 javascript 代码
Puppeteer 最强大的功能是，你可以在浏览器里执行任何你想要运行的 javascript 代码，下面是我在爬 188 邮箱的收件箱用户列表时，发现每次打开收件箱再关掉都会多处一个 iframe 来，随着打开收件箱的增多，iframe 增多到浏览器卡到无法运行，所以我在爬虫代码里加了删除无用 iframe 的脚本：
```javascript
(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    await page.goto('https://webmail.vip.188.com');
    //注册一个 Node.js 函数，在浏览器里运行
    await page.exposeFunction('md5', text =>
        crypto.createHash('md5').update(text).digest('hex')
    );
    //通过 page.evaluate 在浏览器里执行删除无用的 iframe 代码
    await page.evaluate(async () =>  {
        let iframes = document.getElementsByTagName('iframe');
        for(let i = 3; i <  iframes.length - 1; i++){
            let iframe = iframes[i];
            if(iframe.name.includes("frameBody")){
                iframe.src = 'about:blank';
                try{
                    iframe.contentWindow.document.write('');
                    iframe.contentWindow.document.clear();
                }catch(e){}
                //把iframe从页面移除
                iframe.parentNode.removeChild(iframe);
            }
        }
        //在页面中调用 Node.js 环境中的函数
        const myHash = await window.md5('PUPPETEER');
        console.log(`md5 of ${myString} is ${myHash}`);
    });
    await page.close();
    await browser.close();
})();
```

有哪些函数可以在浏览器环境中执行代码呢？

- page.evaluate(pageFunction[, ...args])：在浏览器环境中执行函数
- page.evaluateHandle(pageFunction[, ...args])：在浏览器环境中执行函数，返回 JsHandle 对象
- page.$$eval(selector, pageFunction[, ...args])：把 selector 对应的所有元素传入到函数并在浏览器环境执行
- page.$eval(selector, pageFunction[, ...args])：把 selector 对应的第一个元素传入到函数在浏览器环境执行
- page.evaluateOnNewDocument(pageFunction[, ...args])：创建一个新的 Document 时在浏览器环境中执行，会在页面所有脚本执行之前执行
- page.exposeFunction(name, puppeteerFunction)：在 window 对象上注册一个函数，这个函数在 Node 环境中执行，有机会在浏览器环境中调用 Node.js 相关函数库

### case6: 如何抓取 iframe 中的元素
一个 Frame 包含了一个执行上下文（Execution Context），我们不能跨 Frame 执行函数，一个页面中可以有多个 Frame，主要是通过 iframe 标签嵌入的生成的。其中在页面上的大部分函数其实是 page.mainFrame().xx 的一个简写，Frame 是树状结构，我们可以通过 frame.childFrames() 遍历到所有的 Frame，如果想在其它 Frame 中执行函数必须获取到对应的 Frame 才能进行相应的处理

以下是在登录 188 邮箱时，其登录窗口其实是嵌入的一个 iframe，以下代码时我们在获取 iframe 并进行登录
```javascript
(async () => {
    const browser = await puppeteer.launch({headless: false, slowMo: 50});
    const page = await browser.newPage();
    await page.goto('https://www.188.com');
    //点击使用密码登录
    let passwordLogin = await page.waitForXPath('//*[@id="qcode"]/div/div[2]/a');
    await passwordLogin.click();
    for (const frame of page.mainFrame().childFrames()){
        //根据 url 找到登录页面对应的 iframe
        if (frame.url().includes('passport.188.com')){
            await frame.type('.dlemail', 'admin@admin.com');
            await frame.type('.dlpwd', '123456');
            await Promise.all([
                frame.click('#dologin'),
                page.waitForNavigation()
            ]);
            break;
        }
    }
    await page.close();
    await browser.close();
})();
```
### case7：跳转新 tab 页处理
```javascript
let page = await browser.newPage();
await page.goto(url);
let btn = await page.waitForSelector('#btn');
//在点击按钮之前，事先定义一个 Promise，用于返回新 tab 的 Page 对象
const newPagePromise = new Promise(res => 
  browser.once('targetcreated', 
    target => res(target.page())
  )
);
await btn.click();
//点击按钮后，等待新tab对象
let newPage = await newPagePromise;
```
### case8: 模拟不同的设备
Puppeteer 提供了模拟不同设备的功能，其中 puppeteer.devices 对象上定义很多设备的配置信息，这些配置信息主要包含 viewport 和 userAgent，然后通过函数 page.emulate 实现不同设备的模拟
```javascript
const puppeteer = require('puppeteer');
const iPhone = puppeteer.devices['iPhone 6'];
puppeteer.launch().then(async browser => {
  const page = await browser.newPage();
  await page.emulate(iPhone);
  await page.goto('https://www.google.com');
  await browser.close();
});
```

### 性能和优化
关于共享内存：
```
Chrome 默认使用 /dev/shm 共享内存，但是 docker 默认/dev/shm 只有64MB，显然是不够使用的，提供两种方式来解决：
- 启动 docker 时添加参数 --shm-size=1gb 来增大 /dev/shm 共享内存，但是 swarm 目前不支持 shm-size 参数
- 启动 Chrome 添加参数 - disable-dev-shm-usage，禁止使用 /dev/shm 共享内存

```

- 尽量使用同一个浏览器实例，这样可以实现缓存共用
- 通过请求拦截没必要加载的资源
- 像我们自己打开 Chrome 一样，tab 页多必然会卡，所以必须有效控制 tab 页个数
- 一个 Chrome 实例启动时间长了难免会出现内存泄漏，页面奔溃等现象，所以定时重启 Chrome 实例是有必要的
- 为了加快性能，关闭没必要的配置，比如：-no-sandbox（沙箱功能），--disable-extensions（扩展程序）等
- 尽量避免使用 page.waifFor(1000)，让程序自己决定效果会更好
- 因为和 Chrome 实例连接时使用的 Websocket，会存在 Websocket sticky session 问题，这个需要特别注意











## 网上找到的部分资料
https://www.toutiao.com/article/6891511371465392654/
https://www.zhangshengrong.com/p/7B1LjQGVNw/
https://juejin.cn/post/6957601771694850062
https://chowdera.com/2022/199/202207161109567279.html
https://www.cxymm.net/article/ZHH_Love123/88567702

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
https://juejin.cn/post/6844903680626409486
https://www.shuzhiduo.com/A/QV5ZDwm6zy/
https://zhoushaw.github.io/2019/04/23/learning/node/puppeteer/
https://github.com/Mrlhz/puppeteer
https://static.kancloud.cn/chandler/web_technology/2270683