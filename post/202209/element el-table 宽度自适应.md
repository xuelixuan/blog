# element el-table 宽度自适应

遇到的问题时，当扩大窗口，table 会自适应，但是缩小窗口，table 就不会自适应的问题

解决方案是：

查看 el-table 的父级元素是否有 css 样式设置了 flex：1

一般问题都出在父级元素设置了 flex：1

解决方案两种：

-   删除父级元素的 flex：1，宽度用 calc 属性进行改写。
-   要么在有 flex：1 的父级元素上加上 position:relative
    -   然后在 el-table 的外层增加一层 div，并设置其 position：absolute，width：100%
