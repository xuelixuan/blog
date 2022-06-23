# tinymce获取内容字数并将光标置于文字后面

```JavaScript
// 下面三行代码解决了文章编辑页,数据回填后无法动态获取字数统计的BUG
let ide = tinymce.get(this.tinymceId)
if (!this.isMounted) { // 第一次执行是为了计算字数,后面不需要执行这里的方法
    ide.focus()
    ide.setContent(newValue); // 编辑状态,手动设置内容回填到编辑器
    // 设置内容后,输入文字,光标会置于文字之前
    ide.selection.select(ide.getBody(), true); // 手动设置光标,置于文字之后
    ide.selection.collapse(false)
    this.isMounted = true
}
let wordcount = ide.plugins.wordcount; // 获得编辑器中内容字数
let words = wordcount.body.getCharacterCount()
```