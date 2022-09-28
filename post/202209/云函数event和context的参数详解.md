# 云函数 event 和 context 的参数详解

代码结构

```js
module.exports.main = async((event, context) => {});
```

## event

https://cloud.tencent.com/document/product/583/12513

API 网关触发器的集成请求事件消息结构

```json
{
	"requestContext": {
		"serviceId": "service-f94sy04v",
		"path": "/test/{path}",
		"httpMethod": "POST",
		"requestId": "c6af9ac6-7b61-11e6-9a41-93e8deadbeef",
		"identity": {
			"secretId": "abdcdxxxxxxxsdfs"
		},
		"sourceIp": "10.0.2.14",
		"stage": "release"
	},
	"headers": {
		"accept-Language": "en-US,en,cn",
		"accept": "text/html,application/xml,application/json",
		"host": "service-3ei3tii4-251000691.ap-guangzhou.apigateway.myqloud.com",
		"user-Agent": "User Agent String"
	},
	"body": "{\"test\":\"body\"}",
	"pathParameters": {
		"path": "value"
	},
	"queryStringParameters": {
		"foo": "bar"
	},
	"headerParameters": {
		"Refer": "10.0.2.14"
	},
	"stageVariables": {
		"stage": "release"
	},
	"path": "/test/value",
	"queryString": {
		"foo": "bar",
		"bob": "alice"
	},
	"httpMethod": "POST"
}
```

-   requestContext 请求来源的 API 网关的配置信息、请求标识、认证信息、来源信息。其中：
-   serviceId，path，httpMethod 指向 API 网关的服务 ID、API 的路径和方法。
-   stage 指向请求来源 API 所在的环境。
-   requestId 标识当前这次请求的唯一 ID。
-   identity 标识用户的认证方法和认证的信息。
-   sourceIp 标识请求来源 IP。
-   path 记录实际请求的完整 Path 信息。
-   httpMethod 记录实际请求的 HTTP 方法。
-   queryString 记录实际请求的完整 Query 内容。
-   body 记录实际请求转换为 String 字符串后的内容。
-   headers 记录实际请求的完整 Header 内容。
-   pathParameters 记录在 API 网关中配置过的 Path 参数以及实际取值。
-   queryStringParameters 记录在 API 网关中配置过的 Query 参数以及实际取值。
-   headerParameters 记录在 API 网关中配置过的 Header 参数以及实际取值。

## context

-   memory_limit_in_mb 函数配置内存
-   time_limit_in_ms 函数执行超时时间
-   request_id 函数执行请求 ID
-   environment 函数命名空间信息
-   environ 函数命名空间信息
-   function_version 函数版本信息
-   function_name 函数名称
-   namespace 函数命名空间信息
-   tencentcloud_region 函数所在地域
-   tencentcloud_appid 函数所属腾讯云账号 APPID
-   tencentcloud_uin 函数所属腾讯云账号 ID
