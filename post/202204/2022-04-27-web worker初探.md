# web worker 初探

## 什么是 web worker ？

简单来说，web worker 只不过是一个 javascript 文件，它由浏览器在后台执行，与应用程序 javascript 文件分开。

Web Worker 能够提高应用运行性能。
由于 Web Worker 与主 javascript 文件分开运行，因此可以将主 javascript 文件中存在的计算密集型函数放入 Web Worker 文件中，以便主线程可以顺利运行，而不会在用户界面中出现阻塞。

## 如何设置 Web Worker ？

在项目目录中创建一个 worker.js 文件。

```js
// worker.js
console.log("从 web worker 打印出来的消息!");
this.onmessage = (e) => {
    console.log("worker.js: 从主线程 javascript 接受到的消息", e.data);
    this.postMessage("向主线程发送一条消息：你好");
};
```

`onmessage`：用于监听主应用程序线程发送的任何消息。

`postMessage`：用于向主应用程序线程发送消息。

在 main.js 文件中，可以通过将 worker.js 路径地址传递给 Worker 类来实例化 worker 对象，然后类似地可以使用 onmessage 和 postMessage 函数与工作线程进行通信。

```js
// main.js
const worker = new Worker("worker.js");
worker.postMessage("你好，子线程");
worker.onmessage = (e) => {
    console.log("main.js: 从子线程发送过来的消息:", e.data);
};
// 卸载子线程:
// worker.terminate()
```

可以使用 worker.terminate()函数来终止 web worker 执行。
