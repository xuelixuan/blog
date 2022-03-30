## 使用命名空间

可以用封装原则，将所有数据放在一个预定义且唯一的键或命名空间下。
这将允许我们对应用程序的其他部分（我们无法控制）隐藏我们的信息，并且还可以避免我们错误地更新不应该更新的数据。

第 1 步
创建一个可预测但独一无二的密钥。例如 [your-app-name]+[some-unique-token] 即 DEV-007

第 2 步
在存储信息时，我们从 LocalStorage 读取命名空间值，对其进行反序列化，根据对象内的键更新值，然后在写入 LocalStorage 之前再次对其进行序列化。

第 3 步
在读取信息时，我们从 LocalStorage 中读取命名空间值，对其进行反序列化并从对象中返回键的值。

这样，我们将命名空间视为自己的小 LocalStorage。

下面是上面的代码实现

```js
const NAMESPACE = "DEV-007";

function writeToStorage(key, value) {
    const serializedData = localStorage.getItem(NAMESPACE);
    const data = serializedData ? JSON.parse(serializedData) : {};
    data[key] = value;
    localStorage.setItem(NAMESPACE, JSON.stringify(data));
}

function readFromStorage(key) {
    const serializedData = localStorage.getItem(NAMESPACE);
    const data = JSON.parse(serializedData);
    return data ? data[key] : undefined;
}

function clear() {
    localStorage.setItem(NAMESPACE, JSON.stringify({}));
}

function removeItem(key) {
    const serializedData = localStorage.getItem(NAMESPACE);
    const data = serializedData ? JSON.parse(serializedData) : {};
    delete data[key];
    localStorage.setItem(NAMESPACE, JSON.stringify(data));
}
```

上面的实现 clear 和 removeItem 使用是安全的，解决了我们的问题。

如果使用 npm 包，可以安装`store2`
