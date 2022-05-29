# cloudbase nodejs sdk 操作数据库 聚合查询

官方手册地址：https://docs.cloudbase.net/api-reference/server/node-sdk/database/database

## 聚合查询

person 关联字段 bookId 只显示关联的 id，没有更具体的 book 信息，怎么做才能在查询 person 字段的的时候，bookId 所指的 book 信息也跟着一起查询出来？

```js
const cloudbase = require("@cloudbase/node-sdk");
const app = cloudbase.init({
  env: "",
  secretId: "",
  secretKey: "",
});

const db = app.database();
const $ = db.command.aggregate;

export function getOnePerson(id) {
  return db
    .collection("person")
    .aggregate()
    .lookup({
      from: "book", // book表
      localField: "books", //person表里的books字段
      foreignField: "_id", // book表里的_id
      as: "books", // 查询出来的book信息作为person.books字段进行显示
    })
    .match({
      _id: id, // 根据条件筛选person，person._id = id
    })
    .end();
}
```
