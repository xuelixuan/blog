# vue3 webpack 打包优化

先记录一下项目的开发版本

```js
"@element-plus/icons-vue": "^1.1.4",
"@videojs/http-streaming": "^2.14.2",
"amfe-flexible": "^2.2.1",
"axios": "^0.26.1",
"core-js": "^3.8.3",
"echarts": "^5.3.2",
"echarts-wordcloud": "^2.0.0",
"element-plus": "^2.1.9",
"resize-detector": "^0.3.0",
"video.js": "^6.13.0",
"videojs-contrib-hls": "^5.15.0",
"vue": "^3.2.13",
"vue-router": "^4.0.14",
"vuex": "^4.0.0"
```

`npm install compression-webpack-plugin --save-dev`

打开 vue.config.js

```js
const { defineConfig } = require('@vue/cli-service')
const AutoImport = require('unplugin-auto-import/webpack')
const Components = require('unplugin-vue-components/webpack')
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers')
const CompressionWebpackPlugin = require('compression-webpack-plugin')
const CompressionPlugin = require('compression-webpack-plugin')
const productionGzipExtensions = ['js', 'css']
const isPro = process.env.NODE_ENV === 'production'

const cdn = {
	externals: {
		vue: 'Vue',
		vuex: 'Vuex',
		'vue-router': 'VueRouter',
		echarts: 'echarts',
		axios: 'axios',
		'element-plus': 'ElementPlus',
		videojs: 'videojs',
	},
	css: [
		'https://unpkg.com/element-plus/dist/index.css',
		'https://unpkg.com/video.js/dist/video-js.min.css',
	],
	js: [
		'https://unpkg.com/vue@3.2.13/dist/vue.runtime.global.prod.js',
		'https://unpkg.com/vue-router@4.0.14/dist/vue-router.global.prod.js',
		'https://unpkg.com/video.js/dist/video.min.js',
		'https://unpkg.com/vuex@4.0.0/dist/vuex.global.prod.js',
		'https://unpkg.com/element-plus@2.2.0/dist/index.full.min.js',
		'https://unpkg.com/echarts@5.3.2/dist/echarts.min.js',
		'https://unpkg.com/axios@0.27.2/dist/axios.min.js',
	],
}

module.exports = defineConfig({
	publicPath: './',
	transpileDependencies: true,
	lintOnSave: false,
	productionSourceMap: isPro ? false : true,

	devServer: {
		proxy: {
			'/api': {
				target: '',
				ws: true,
				secure: false,
				changeOrigin: true,
				pathRewrite: {
					'/': '/',
				},
			},
		},
	},
	// 更改index.html
	chainWebpack: (config) => {
		config.plugin('html').tap((args) => {
			args[0].title = '网站要显示的标题'
			isPro ? (args[0].cdn = cdn) : null
			return args
		})
	},

	// scss loader
	pluginOptions: {
		'style-resources-loader': {
			preProcessor: 'scss',
			patterns: [],
		},
	},

	configureWebpack: {
		plugins: [
			AutoImport({
				resolvers: [ElementPlusResolver()],
			}),
			Components({
				resolvers: [ElementPlusResolver()],
			}),
			new CompressionWebpackPlugin({
				algorithm: 'gzip',
				test: new RegExp(
					'\\.(' + productionGzipExtensions.join('|') + ')$'
				),
				threshold: 10240,
				minRatio: 0.8,
			}),
			new CompressionPlugin({
				test: /\.(js|css)(\?.*)?$/i, //需要压缩的文件正则
				threshold: 10240, //文件大小大于这个值时启用压缩
				deleteOriginalAssets: false, //压缩后保留原文件
			}),
		],

		externals: isPro ? cdn.externals : {},
	},
})
```
