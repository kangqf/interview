## 一些 GitBook 的插件使用

### book.json
书籍根目录下面的 book.json 可以用来配置书籍用的插件和样式，其配置方法参考[官方文档](https://toolchain.gitbook.com/config.html)

其中[http://www.chengweiyang.cn/gitbook/customize/book.json.html](http://www.chengweiyang.cn/gitbook/customize/book.json.html)给出了一份示例的配置文件
``` json
{
    "author": "Chengwei Yang <me@chengweiyang.cn>",
    "description": "This is a sample book created by gitbook",
    "extension": null,
    "generator": "site",
    "isbn": null,
    "links": {
        "sharing": {
            "all": null,
            "facebook": null,
            "google": null,
            "twitter": null,
            "weibo": null
        },
        "sidebar": {
            "Chengwei's Blog": "http://www.chengweiyang.cn"
        }
    },
    "output": null,
    "pdf": {
        "fontSize": 12,
        "footerTemplate": null,
        "headerTemplate": null,
        "margin": {
            "bottom": 36,
            "left": 62,
            "right": 62,
            "top": 36
        },
        "pageNumbers": false,
        "paperSize": "a4"
    },
    "plugins": [],
    "title": "Sample GitBook",
    "variables": {}
}
```

### 插件使用
1. [gitbook-plugin-highlight-pro](https://github.com/tkggcelt/gitbook-plugin-highlight-pro) 
一款改善gitbook代码高亮的插件，配置为：
    ``` json
    {
        "plugins": [
            "-highlight", 
            "highlight-pro"
        ],
    }
    ```
2. [gitbook-theme-comscore](https://www.npmjs.com/package/gitbook-theme-comscore) 
ComScore 是一个彩色主题插件，把原来黑白的界面变得鲜艳了。

