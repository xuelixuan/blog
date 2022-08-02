# sessionStorage 跨浏览器标签共享

```javascript
(function () { // 跨窗口共享sessionStorage
    //这里面的token是你存在sessionStorage里面的用户标识的key值
    if (!sessionStorage.length) {
        // 这个调用能触发目标事件，从而达到共享数据的目的
        localStorage.setItem('token', Date.now());
    };
    // 该事件是核心
    window.addEventListener('storage', function (event) {
        if (event.key == 'token') {
            // 已存在的标签页会收到这个事件
            localStorage.setItem('sessionStorage', JSON.stringify(sessionStorage));
            localStorage.removeItem('sessionStorage');

        } else if (event.key == 'sessionStorage' && !sessionStorage.length) {
            // 新开启的标签页会收到这个事件
            var data = JSON.parse(event.newValue);

            for (let key in data) {
                sessionStorage.setItem(key, data[key]);
            }
        }
        //可以在这里写你是否要免登录的逻辑
    });
})();
```