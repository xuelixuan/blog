# nuxt2 + tailwind.css 环境搭建

第一步,安装 nuxt

`npx create-nuxt-app <项目名>`

安装过程中选择 tailwind

安装成功后,停止运行 nuxt 项目,继续安装 tailwind

第二步,安装 tailwind

`yarn add -D tailwindcss@latest postcss@latest autoprefixer@latest`

安装成功后,nuxt 项目根目录下创建`tailwind.config.js`,或在终端中执行命令`npx tailwindcss init`

打开 tailwind.config.js

```js
module.exports = {
	purge: [
		'./components/**/*.{vue,js}',
		'./submodules/**/*.{vue,js}',
		'./layouts/**/*.vue',
		'./pages/**/*.vue',
		'./plugins/**/*.{js,ts}',
		'./nuxt.config.{js,ts}',
	],
	// darkMode: false, // or 'media' or 'class'
	prefix: '',
	theme: {
		extend: {},
	},
	variants: {
		extend: {},
	},
	plugins: [],
	important: true,
}
```

第三步,nuxt 根目录下新建`assets/css/`目录,并新建文件`assets/css/tailwind.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

第四步,打开`nuxt.config.js`,修改 css 字段

```js
    // Global CSS: https://go.nuxtjs.dev/config-css
    css: ["~/assets/css/tailwind.css"],
```

第五步,重启 nuxt 项目.

在`pages/index.vue`文件写入 tailwind 样式,查看效果.

## 版本号留存

```js
  "dependencies": {
    "@nuxtjs/axios": "^5.13.6",
    "core-js": "^3.19.3",
    "nuxt": "^2.15.8",
    "vue": "^2.6.14",
    "vue-server-renderer": "^2.6.14",
    "vue-template-compiler": "^2.6.14",
    "webpack": "^4.46.0"
  },
  "devDependencies": {
    "@babel/eslint-parser": "^7.16.3",
    "@nuxtjs/eslint-config": "^8.0.0",
    "@nuxtjs/eslint-module": "^3.0.2",
    "@nuxtjs/tailwindcss": "^4.2.1",
    "autoprefixer": "^10.4.4",
    "eslint": "^8.4.1",
    "eslint-config-prettier": "^8.3.0",
    "eslint-plugin-nuxt": "^3.1.0",
    "eslint-plugin-vue": "^8.2.0",
    "postcss": "^8.4.12",
    "prettier": "^2.5.1",
    "tailwindcss": "^3.0.24"
  }
```
