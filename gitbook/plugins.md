## 一些 GitBook 的插件使用

### book.json
书籍根目录下面的 book.json 可以用来配置书籍用的插件和样式，其配置方法参考[官方文档](https://toolchain.gitbook.com/config.html)
### 插件使用
1. [gitbook-plugin-highlight-pro](https://github.com/tkggcelt/gitbook-plugin-highlight-pro) 一款改善gitbook代码高亮的插件

配置为
``` json
{
    "plugins": [
        "-highlight", 
        "highlight-pro"
    ],
}
```

