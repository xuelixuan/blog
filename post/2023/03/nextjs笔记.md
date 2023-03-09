# nextjs 笔记

http://nextjs-in-action-cn.taonan.lu/
https://nextjs.zhd.icu/

## 资源、元数据和 CSS

## 元数据

## 公共样式

在/styles 目录下新建 global.css 文件
在/pages 目录下新建\_app.js 文件

```js
import '../styles/global.css'
export default function App({ Component, pageProps }) {
    return <Component {...pageProps} />
}
```

css 穿透
当前组件 css 穿透到 antd 这样的第三方组件，实现在组件内更改 antd,swiper 等插件样式。
使用 :global 关键字

```js
.swiper {
    :global span.swiper-pagination-bullet {
        border-radius: 0 !important;
        width: 30px;
        height: 4px;
        background-color: #c7c7c7;
        opacity: 1;
    }
}
```

## 内置组件

### next/router

如果想要在应用程序中访问 router 对象，可以使用 useRouter 钩子

```js
import { useRouter } from 'next/router'
function ActiveLink({ children, href }) {
    const router = useRouter()
    const style = {
        marginRight: 10,
        color: router.asPath === href ? 'red' : 'black',
    }

    const handleClick = (e) => {
        e.preventDefault()
        router.push(href)
    }

    return (
        <a href={href} onClick={handleClick} style={style}>
            {children}
        </a>
    )
}

export default ActiveLink
```

router 对象的详细参数以及用法，可以直接查看官方文档 ↓
来源:https://nextjs.org/docs/api-reference/next/router https://nextjs.org/learn/basics/navigate-between-pages/link-component

### next/link

在客户端进行路由之间的切换。
使用该组件比原生 a 标签更具优势，导航更快：
●a 链接会重新加载整个页面
●Link 组件不会重新加载整个页面
优势说明：https://nextjs.org/learn/basics/navigate-between-pages/client-side
Link 接受很多参数，这里只介绍一个最常用的属性 href
href 属性的值可以是一个 URL 链接，也可以是一个对象 Object

```js
import Link from 'next/link'

function Home() {
  return (
    <ul>
      <li>
        <Link href="/">Home</Link>
      </li>
      <li>
        <Link href="/about">About Us</Link>
      </li>
      <li>
        <Link href="/blog/hello-world">Blog Post</Link>
      </li>
    </ul>
  )
}

export default Home

import Link from 'next/link'
function Home() {
  return (
    <ul>
      <li>
        <Link
          href={{
            pathname: '/about',
            query: { name: 'test' },
          }}
        >
          About us
        </Link>
      </li>
      <li>
        <Link
          href={{
            pathname: '/blog/[slug]',
            query: { slug: 'my-post' },
          }}
        >
          Blog Post
        </Link>
      </li>
    </ul>
  )
}
export default Home
```

来源：https://nextjs.org/docs/api-reference/next/link https://nextjs.org/learn/basics/navigate-between-pages/link-component

### next/image

该组件比原生 img 标签更具优势，开箱即用：
● 确保您的图像在不同的屏幕尺寸上响应
● 使用第三方工具或库优化图像
● 仅在图像进入视口时加载图像

```js
import Image from 'next/image'

const YourComponent = () => (
    <Image
        src="/images/profile.jpg" // Route of the image file
        height={144} // Desired size with correct aspect ratio
        width={144} // Desired size with correct aspect ratio
        alt="Your Name"
    />
)
```

图像优化
https://nextjs.org/docs/basic-features/image-optimization

来源：https://nextjs.org/docs/api-reference/next/image https://nextjs.org/learn/basics/assets-metadata-css/assets

### next/head

可以将元素添加到 html 的 head 标签内。

```js
import Head from 'next/head'

function IndexPage() {
    return (
        <div>
            <Head>
                <title>My page title</title>
                <meta property="og:title" content="My page title" key="title" />
            </Head>
            <p>Hello world!</p>
        </div>
    )
}

export default IndexPage
```

来源：https://nextjs.org/docs/api-reference/next/head

### next/script

## 生命周期

### 组件的生命周期

useEffect 是一个内置的 React Hook。可以用来作为组件的生命周期函数。
用法如下：

```js
import { useEffect } from 'react'

useEffect(() => {
    // 每次渲染组件都会执行
})

useEffect(() => {
    // 只在组件第一次渲染时执行
}, [])

useEffect(() => {
    // 通过将依赖项数组作为第二个参数传递给useEffect，useEffect将仅在依赖项更改时运行（count变量一旦更改，就会执行一次useEffect函数，充当watch）
}, [count])

useEffect(() => {
    // 仅首次渲染组件时执行一次 or 依赖项变更时执行
    return () => {
        // 卸载（销毁）组件之前执行。
    }
}, [prop, state])
```

// 每个组件内可以多次使用 useEffect 函数，来达到不同的生命周期目的。

## 数据传递

组件和组件之间的数据传递。
四种方式： 1.通过 props

```js
const Component = (props) => {
    // You can get data from parent component.
    const parentData = props.parentData
    useEffect(() => {
        // Fetch Data here when component first mounted
    }, [])
}
```

2.使用 React 的上下文传

```js
function ContainerOfTwo(props) {
    const [sharedState, setSharedState] = React.useState({})
    return (
        <>
            <MyContext.Provider value={[sharedState, setSharedState]}>
                <ChildOne />
                <ChildTwo />
            </MyContext.Provider>
        </>
    )
}
```

3.使用状态提升

```js
function ParentComponent(props) {
    const initialState = {}
    const [sortedData, setSortedData] = React.useState(initialState)
    return (
        <>
            <ChildComponent data={sortedData} setData={setSortedData} />
        </>
    )
}
```

4.使用全局状态管理工具

```js
class Parent extends Component {
    constructor(props) {
        super(props)
        this.state = {
            products: [],
        }
    }
    callBackMethod = (products) => {
        this.setState({ products })
    }
    render() {
        const { products } = this.state
        return (
            <>
                <ChildComponent products={products} callBackMethod={this.callBackMethod} />
            </>
        )
    }
}
```

### 数据监听

与 vuejs 里的 watch 功能类似，react 监听数据变化的方法如下：

```js
import React, { useEffect, useState } from 'react'

function MyComponent(props) {
    const [myData, setMyData] = useState([])

    useEffect(() => {
        // 订阅 myData 数值变化
        myData.on('change', () => {
            console.log('myData changed!')
        })

        // 在组件unmount时，销毁订阅事件
        return () => {
            myData.off('change')
        }
    }, [myData])

    return <div>My Component</div>
}
```

## 数据渲染

### 页面级：静态生成（静态页面）

来源：

### 页面级：动态渲染（服务端渲染页面）

来源：https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props

组件级：客户端数据（浏览器中通过 JS 动态获取数据）
在自己封装的组件中获取后端数据，只能通过下列 2 种方式：
使用 useEffect 获取客户端数据

```js
import { useState, useEffect } from 'react'

function Profile() {
    const [data, setData] = useState(null)
    const [isLoading, setLoading] = useState(false)

    useEffect(() => {
        setLoading(true)
        fetch('/api/profile-data')
            .then((res) => res.json())
            .then((data) => {
                setData(data)
                setLoading(false)
            })
    }, [])

    if (isLoading) return <p>Loading...</p>
    if (!data) return <p>No profile data</p>

    return (
        <div>
            <h1>{data.name}</h1>
            <p>{data.bio}</p>
        </div>
    )
}
```

### 使用 SWR 获取客户端数据 （强烈建议）

SWR 可以处理缓存、重新验证、焦点跟踪、按时间间隔重新获取等等

```js
import useSWR from 'swr'

const fetcher = (...args) => fetch(...args).then((res) => res.json())

function Profile() {
    const { data, error } = useSWR('/api/profile-data', fetcher)

    if (error) return <div>Failed to load</div>
    if (!data) return <div>Loading...</div>

    return (
        <div>
            <h1>{data.name}</h1>
            <p>{data.bio}</p>
        </div>
    )
}
```

来源：https://nextjs.org/docs/basic-features/data-fetching/client-side

### 增量静态再生成：动态生成静态页

当对一个在构建时已预渲染的页面提出请求时，它最初将显示缓存的页面。

-   在最初的请求之后和 10 秒之前，任何对该页面的请求都是缓存且即时的
-   在 10 秒之后，下一次请求仍然会显示缓存的（陈旧的）页面
-   Next.js 会在后台触发页面的再生（regeneration）
-   一旦页面再生成功，Next.js 将使缓存失效并显示更新的页面；如果后台再生失败，旧的页面不会被改变

当对一个尚未生成的路径提出请求时，Next.js 将在第一次请求中对页面进行服务端渲染，之后的请求将从缓存中提供静态文件。

第一步：配置重新生成静态页面时间

```js
export async function getStaticProps() {
    const res = await fetch('https://api.example.com/data')
    const data = await res.json()

    return {
        props: {
            data,
        },
        revalidate: 10, // 10s后重新生成静态静态页面
    }
}
```

第二步：使用 useEffect 钩子获取数据。当页面被访问时，当前页面拿到的是最新数据，同时 nextjs 会在后台按照 revalidate 指定的时间，异步重新生成当前页面。

```js
import { useState, useEffect } from 'react'

export default function MyPage({ data }) {
    const [latestData, setLatestData] = useState(data)

    useEffect(() => {
        async function fetchLatestData() {
            const res = await fetch('https://api.example.com/data')
            const latestData = await res.json()
            setLatestData(latestData)
        }
        const intervalId = setInterval(fetchLatestData, 10000) // fetch new data every 10 seconds
        return () => clearInterval(intervalId)
    }, [])

    return (
        <div>
            <h1>My Page</h1>
            <p>{latestData}</p>
        </div>
    )
}
```

第三步：如果页面使用了动态路由，需要用 getStaticPaths 函数来指定需要预渲染的页面。

```js
export async function getStaticPaths() {
    const res = await fetch('https://api.example.com/posts')
    const posts = await res.json()

    const paths = posts.map((post) => ({
        params: { id: post.id },
    }))

    return {
        paths,
        fallback: 'blocking', // 动态路径不会提前渲染静态页面，会在用户访问的一瞬间，同时生成静态页，一旦生成完成，会直接把静态页返回给用户。之后的用户如果再次访问当前路径，会直接把已经生成过的静态页面返回给用户。
    }
}

export async function getStaticProps({ params }) {
    const res = await fetch(`https://api.example.com/posts/${params.id}`)
    const post = await res.json()

    return {
        props: {
            post,
        },
        revalidate: 10, // 当前页面一旦被访问，会在10s之后重新生成当前页。
    }
}

export default function Post({ post }) {
    return (
        <div>
            <h1>{post.title}</h1>
            <p>{post.content}</p>
        </div>
    )
}
```

### 渲染 HTML(文章内容)

与 vuejs 的 v-html 功能类似.react 渲染 HTML 的方法如下：
`<div dangerouslySetInnerHTML={{ __html: post.content }}></div>`
post.content 是要渲染的字段。

## 路由配置

### 动态路由

文件系统就是路由，创建文件就等于创建了路由。
配置路由的方式就是在/pages 目录下新建文件:
`/pages/index.js` 指向 `http://localhost:3000/`
`/pages/about.js` 指向 `http://localhost:3000/about`
`/pages/report/index.js` 指向 `http://localhost:3000/report`
`/pages/report/list.js` 指向 `http://localhost:3000/report/list`
`/pages/report/[id].js` 指向 `[http://localhost:3000/report/123 或 http://localhost:3000/report/abc]`

来源：https://nextjs.org/learn/basics/navigate-between-pages/pages-in-nextjs

### 路由对象

通过路由对象获取当前页面路由参数

```js
import { useRouter } from 'next/router'

function Report() {
    const router = useRouter()
    const { id } = router.query
    return <div>报告详情页: {id}</div>
}

export default Report
```

### API 路由

暂时用不到。

## 开发部署

创建 Next.js 应用程序
npx create-next-app@latest project-name --use-npm --example "https://github.com/vercel/next-learn/tree/master/basics/learn-starter"

### 配置环境变量

无需下载 cross-env 插件，nextjs 内置对环境变量配置文件的支持。直接创建文件即可。
开发环境创建.env.development.local 文件；生产环境创建.env.production.local 文件。

```js
# .env.development.local 文件内写入
NODE_ENV=development
BASE_URL=https://api2.chooseauto.com

# .env.production.local 文件内写入
NODE_ENV=production
BASE_URL=https://api.chooseauto.com
```

根据配置好的环境变量，可以直接在代码中通过 process.env.变量名的方式引入变量。
nextjs 在执行 npm run dev 时，process.env.NODE*ENV=development；
nextjs 在执行 npm run start 时，process.env.NODE_ENV=production。
根据这个特性，可以实现不同环境，不同配置。
注意：配置好的环境变量，默认只能在 node 环境下被读取，如果想要在 node 端和浏览器端读取同一个环境变量，需要添加 NEXT_PUBLIC*前缀。

```js
# .env.production.local
NEXT_PUBLIC_SEARCH_URL=https://it.chooseauto.com.cn
# 这个环境变量既可以在node端被使用，也可以在浏览器端被使用
SEARCH_URL=https://it.chooseauto.com.cn
# 这个环境变量只能在node端被使用，浏览器端会返回undefined
```

### 启动项目（开发环境）

`npm run dev`

### 构建项目（生产环境）

会根据代码逻辑提前生成一部分静态页面或服务端渲染页面
`npm run build`

### 启动 Node 服务器

为静态生成的页面和服务端渲染的页面提供服务
`npm run start`

也可以自定义 Node 服务端口号，只要修改 package.json
`"start": "next start -p 9999"`

这样，Node 服务就会以 9999 为端口号进行启动:http://localhost:9999

## 项目配置

### CSS 兼容 IE11

nextjs 内置 `autoprefixer`，但需要手动配置后才能生效。
打开 `package.json` 文件，添加 `browserslist` 字段：

```json
{
    "private": true,
    "scripts": {
        "dev": "next dev",
        "build": "next build",
        "start": "next start"
    },
    "browserslist": ["last 2 versions", "not ie < 11", "not op_mini all"]
}
```

之后再次运行 `npm run build` 命令，nextjs 会自动将 css 的代码进行 `autoprefixer`，兼容 IE。

### 项目兼容 IE11

项目中使用最新 js 语法，打包后可以兼容 IE11
项目根目录创建`.babelrc` 文件

```json
{
    "presets": [
        [
            "next/babel",
            {
                "preset-env": {
                    "useBuiltIns": "usage",
                    "modules": false,
                    "debug": false,
                    "corejs": 3,
                    "targets": {
                        "browsers": ["last 2 versions", "ie >= 11"]
                    }
                }
            }
        ]
    ]
}
```

安装 core-js
`npm install core-js --save-dev`

重启项目可以正常进行
`npm run dev`

打包过程，会把 es6 转成 es5，完成对 IE11 的支持
`npm run build`

### Antd 兼容 IE11

antd 5 已经不再支持 IE 浏览器，通过查阅资料以及尝试，没有发现更好的办法，只能在版本上做降级处理，从 v5 降到 v4。
v4 文档地址：https://4x.ant.design/components/overview-cn/
安装 antd v4
`npm install antd@4`

\_app.js 中引入样式
`import 'antd/dist/antd.css'`;

### Antd 汉化

antd 默认是英文版，需要进行汉化配置。但配置后只做到了部分汉化，日期插件汉化需要额外进行配置。
antd 日期选择器汉化必须引入 moment 中文。
在\_app.js 中进行汉化配置。

```js
import '../styles/globals.css'
import 'antd/dist/antd.css'
import zhCN from 'antd/lib/locale/zh_CN'
import { ConfigProvider } from 'antd'
import 'moment/locale/zh-cn'
export default function App({ Component, pageProps }) {
    return (
        <ConfigProvider locale={zhCN}>
            <Component {...pageProps} />
        </ConfigProvider>
    )
}
```

### Antd 主题配置

antd v4 使用 less 开发，但 nextjs 默认只支持 sass。
所以要先安装 next-with-less 插件：
`npm install next-with-less --save-dev`

next.config.js 中写入

```js
const withLess = require('next-with-less')
const path = require('path')

const antd_custom_theme = path.resolve(__dirname, './styles/antd-custom-theme.less')

module.exports = withLess({
    lessLoaderOptions: {
        additionalData: (content) => `${content}\n\n@import '${antd_custom_theme}';`,
    },
    // next.config.js的其他配置项直接从这个位置往下写
    ...
})

root/styles/antd-custom-theme.less中进行自定义主题配置。
```
