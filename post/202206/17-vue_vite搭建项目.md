# vue vite 搭建项目

创建新项目
`npm init vite@latest <project-name> -- --template vue`

按需加载 element plus
`npm install -D unplugin-vue-components unplugin-auto-import`

然后把下列代码插入到你的 `Vite` 的配置文件中

```js
// vite.config.ts
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

export default defineConfig({
	// ...
	plugins: [
		// ...
		AutoImport({
			resolvers: [ElementPlusResolver()],
		}),
		Components({
			resolvers: [ElementPlusResolver()],
		}),
	],
})
```

引入 sass

`npm install sass -D`
然后 vue 文件中就可以直接使用 scss 语法

全局引入 scss 文件,需要配置 vite.config.js

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

// https://vitejs.dev/config/
export default defineConfig({
	plugins: [
		vue(),
		AutoImport({
			resolvers: [ElementPlusResolver()],
		}),
		Components({
			resolvers: [ElementPlusResolver()],
		}),
	],
	resolve: {
		alias: {
			'@': '/src',
		},
	},
	css: {
		//css预处理
		preprocessorOptions: {
			scss: {
				/*
                引入var.scss全局预定义变量，
                如果引入多个文件，
                可以使用
                '@import "@/assets/scss/globalVariable1.scss";@import "@/assets/scss/globalVariable2.scss";'
                这种格式
                 */
				additionalData: '@import "@/assets/scss/global.scss";',
			},
		},
	},
})
```

引入 vue-router
`npm install vue-router@4`

```js
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
	{
		path: '/',
		redirect: '/abcd',
	},
]

const router = createRouter({
	linkActiveClass: 'active',
	routes,
	history: createWebHashHistory('/'),
})

router.beforeEach((to, from, next) => {
	const title = to.meta && to.meta.title
	if (title) {
		document.title = title
	}
	next()
})

export { routes }
export default router
```

引入 vuex [用法](https://cloud.tencent.com/developer/article/1989548)
`npm install vuex@next --save`
