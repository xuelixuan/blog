# 一些关于 RSS 资源的阅读片段

-   [Transform RSS Feed into API using Express JS](https://lioncoding.com/transform-rss-feed-into-api-using-express-js/)
    -   基本框架的搭建，提供了单一 RSS 源的解析逻辑，没有批量解析逻辑。
-   [Front-End RSS](https://github.com/ChanceYu/front-end-rss)
    -   nodejs 抓取-vue 展示-抓取标题自动更新到 git 上
    -   Later.js 使用重复计划的最简单方法 http://bunkat.github.io/later/

```js
// app.js

const app = require("later");
const handleUpdate = require("./update");

later.date.localTime(); // 设置使用服务器时间
later.setInterval(handleUpdate, {
	// 每天6，8，12，18，22点进行抓取
	schedules: [
		{ h: [06], m: [00] },
		{ h: [08], m: [00] },
		{ h: [12], m: [00] },
		{ h: [18], m: [00] },
		{ h: [22], m: [00] },
	],
});
```

fetch.js

```js
const Parser = require("rss-parser");
const Async = require("async");

const utils = require("./utils");

require("dotenv").config();

async function fetchFeed(rss) {
	// 实例化Parser对象，用该对象抓取RSS
	const parser = new Parser({
		headers: {
			"User-Agent":
				"Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1",
		},
	});
	// 如果rss抓取信息成功，返回feed对象，否则返回true
	try {
		const feed = await parser.parseURL(rss);
		if (feed) {
			utils.logSuccess("成功 RSS: " + rss);
			return feed;
		}
	} catch (e) {}

	utils.logWarn("失败 RSS: " + rss);
	return true;
}

// 初始化fetch方法，rssItem是单个网站的rss feed
async function initFetch(rssItem, onFinish) {
	const envRss = process.env["RSS_" + rssItem.id];
	let rssArray = rssItem.rss; // rss link

	if (typeof rssArray === "string") {
		rssArray = [rssArray];
	}

	if (envRss) {
		rssArray.unshift(envRss);
	}
	// 将所有更新RSS源的方法，插入到数组当中
	const tasks = rssArray.map((rss) => (callback) => {
		(async () => {
			const feed = await fetchFeed(rss);

			if (feed === true) {
				callback(true);
			} else {
				callback(null, feed);
			}
		})();
	});

	utils.log("开始 RSS: " + rssItem.title);

	return new Promise((resolve) => {
		// tryEach方法会尝试迭代执行tasks数组中的方法，并传入一个callback，res会返回feed数据对象。
		// 只要taskts中的函数成功执行一次，就会调用一次callback，或者所有函数都执行失败，最后一次执行失败时调用callback
		Async.tryEach(tasks, (err, res) => {
			utils.log("完成 RSS: " + rssItem.title);
			resolve(err ? null : res); // resolve的是一个网站的所有rss link 对象
		});
	});
}

module.exports = initFetch;
```

看到这里https://github.com/ChanceYu/front-end-rss/blob/master/server/update.js
