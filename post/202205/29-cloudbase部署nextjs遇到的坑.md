# cloudbase 部署 nextjs 遇到的坑

根据 cloudbase 默认推荐的应用模板，选择线上安装 nextjs ssr。
安装完成后发现版本太低，结果升级后上传云函数直接报错。
这个问题搞了三四天，一直没想明白问题到底出在哪。

最后观察云函数列表发现问题的根源：

按照官方推荐模板安装，云函数运行的环境是 node 10.15
但是自己手动创建云函数，云函数运行的环境是 node 12.16

问题的根源就是因为 nextjs 是最新版，不支持在 node 10.15 的环境下运行.

最后的解决方案：

手动创建一个云函数，这样云函数的运行环境就是 12.16

然后在 vscode 中安装 cloudbase toolkit 插件。

登录方式选择密钥方式登录。

（在腾讯云管理后台，右上角头像-选择访问管理-密钥访问，拿到 sid 和 skey）

登录成功后，更新一下云函数列表-选择刚才创建的云函数-右键代码下载到本地。

把自己本地开发的 nextjs 程序覆盖进刚刚创建的云函数目录内。

然后把整个文件夹更新到线上，问题得到解决。

## 细节

1. 云函数根目录要创建 index.js

```js
/**
 * Tencent is pleased to support the open source community by making CloudBaseFramework - 云原生一体化部署工具 available.
 *
 * Copyright (C) 2020 THL A29 Limited, a Tencent company.  All rights reserved.
 *
 * Please refer to license text included with this package for license details.
 */
require = require("esm")(module);
const path = require("path");
const Koa = require("koa");
const next = require("next");
const serverless = require("serverless-http");

async function main(...args) {
  const event = args[0];
  // 针对部署在子路径的情况需要手动带上路径前缀
  event.path = path.join("/nextjs", event.path);

  const dev = false;
  const app = next({ dev });
  const handle = app.getRequestHandler();
  const server = new Koa();
  await app.prepare();

  server.use((ctx) => {
    ctx.status = 200;
    ctx.respond = false;
    try {
      if (ctx.path === "/nextjs/") {
        app.render(ctx.req, ctx.res, "/index", ctx.query);
      } else {
        handle(ctx.req, ctx.res);
      }
    } catch (e) {
      console.log(e);
    }
  });

  return serverless(server, {
    binary: [
      "application/javascript",
      "application/json",
      "application/octet-stream",
      "application/xml",
      "font/eot",
      "font/opentype",
      "font/otf",
      "image/*",
      "video/*",
      "audio/*",
      "text/comma-separated-values",
      "text/css",
      "text/javascript",
      "text/plain",
      "text/text",
      "text/xml",
    ],
  })(...args);
}

exports.main = main;
```

该文件内的/nextjs 路径与腾讯云后台配置的 http 访问路径一致。

(要想通过 http 访问，必须把 http 访问的路径和创建的云函数进行关联绑定-cloudbase 后台-环境-访问服务进行配置)

2. nextjs 的 package.json 文件

```json
{
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "@cloudbase/node-sdk": "^2.9.1",
    "esm": "^3.2.25",
    "koa": "^2.13.4",
    "next": "12.1.6",
    "path": "^0.12.7",
    "react": "17.0.2",
    "react-dom": "17.0.2",
    "serverless-http": "^3.0.1"
  },
  "devDependencies": {
    "autoprefixer": "^10.4.7",
    "postcss": "^8.4.14",
    "tailwindcss": "^3.0.24"
  }
}
```

3. next.config.js 文件

```js
module.exports = {
  basePath: "/nextjs",
  assetPrefix: "/nextjs",
};
```

next.config.js 文件内的/nextjs 路径要与 index.js 内的路径一致，要与腾讯云后台 cloudbase 配置 http 访问的子路径一致

4. 本地 nextjs 项目作为云函数上传之前，要在本地先执行 npm run build 重新生成.next/文件夹

5. SSG 还是 SSR 是由 nextjs 本身来决定的，取决于在代码中使用 getStaticPaths，getStaticProps 还是使用了 getServerSideProps.

具体是 SSG 还是 SSR 根据【nextjs 的 SSG 和 SSR】这篇文章来做具体代码上的实现.（博客首页搜索这篇标题）
