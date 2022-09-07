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

update.js

```js
const fs = require("fs-extra"); // 添加了node fs模块没有的方法，支持promise
const Async = require("async"); // 异步库
const moment = require("moment"); // 时间格式
const Git = require("simple-git"); // 自动提交到git

const utils = require("./utils");
const writemd = require("./writemd");
const fetch = require("./fetch");

const { RESP_PATH, RSS_PATH, LINKS_PATH } = utils.PATH;

let rssJson = null;
let linksJson = null;
let newData = null;

/**
 * 更新 git 仓库
 */
function handleUpdate() {
	utils.log("开始更新抓取");

	Git(RESP_PATH).pull().exec(handleFeed);
}

/**
 * 提交修改到 git 仓库
 */
function handleCommit() {
	utils.log("完成抓取，即将上传");

	Git(RESP_PATH)
		.add("./*")
		.commit("更新: " + newData.titles.join("、"))
		.push(["-u", "origin", "master"], () => utils.logSuccess("完成抓取和上传！"));
}

/**
 * 处理订阅源
 */
function handleFeed() {
	rssJson = fs.readJsonSync(RSS_PATH);
	linksJson = fs.readJsonSync(LINKS_PATH);
	newData = {
		length: 0,
		titles: [],
		rss: {},
		links: {},
	};

	const tasks = rssJson.map((rssItem, rssIndex) => (callback) => {
		(async () => {
			const feed = await fetch(rssItem); // 返回单一源的所有list
			if (feed) {
				const items = linksJson[rssIndex].items || []; // 拿到本地list
				const newItems = feed.items.reduce((prev, curr) => {
					// prev 是上一次reduce执行函数得到的结果
					const exist = items.find((el) => utils.isSameLink(el.link, curr.link)); // 两个list进行对比
					if (exist) {
						// 已经有了，直接跳过这一条，进行下一条
						return prev;
					} else {
						// 如果没有，进行信息录入
						let date = moment().format("YYYY-MM-DD");

						try {
							date = moment(curr.isoDate).format("YYYY-MM-DD");
						} catch (e) {}

						newData.rss[rssItem.title] = true;
						newData.links[curr.link] = true;

						return [
							...prev,
							{
								title: curr.title,
								link: curr.link,
								date,
							},
						];
					}
				}, []);
				// 如果有更新
				if (newItems.length) {
					utils.logSuccess("更新 RSS: " + rssItem.title);
					newData.titles.push(rssItem.title);
					newData.length += newItems.length;
					// 将新增的数据添加到linksJson
					linksJson[rssIndex] = {
						title: rssItem.title,
						items: newItems.concat(items).sort(function (a, b) {
							return a.date < b.date ? 1 : -1;
						}),
					};
				}
			}
			callback(null);
		})();
	});
	// 异步请求所有源，如果有新增，就写入到文件，并更新md，并提交到github
	Async.series(tasks, async () => {
		if (newData.length) {
			fs.outputJsonSync(LINKS_PATH, linksJson, {
				spaces: 2,
			});
			await writemd(newData, linksJson);
			handleCommit();
		} else {
			utils.logSuccess("无需更新");
		}
		rssJson = null;
		linksJson = null;
		newData = null;
	});
}

module.exports = handleUpdate;
```
