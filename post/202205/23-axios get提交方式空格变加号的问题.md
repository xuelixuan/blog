# axios get 提交方式空格变加号的问题

在用 axios 给后端提交数据的时候，默认使用的是如下方法

```js
export function createTask(params) {
    return http({
        url: rapUrl + "/saveTask/",
        method: "get",
        params,
    });
}
```

在数据提交过程发现一个问题，params 字段中有个时间字符串'2022-01-01 18:00'，
在提交到后端的过程中被转码成了'2022-01-01+18:00'.

空格变成了+号。

解决这个问题的办法是把请求参数直接写死在请求 url 上

```js
export function createTask(params) {
    return http({
        url: rapUrl + "/saveTask/?param=" + JSON.stringify(params.param),
        method: "get",
        params,
    });
}
```

这样，提交过程中，遇到空格就会自动转换成 20.问题解决。
