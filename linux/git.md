## git 的日常使用

1. 设置git diff比较的文件
使用`--diff-filter = M`限制diff输出只有内容修改。
可以使用`git attributes` 来限制要比较的文件。创建一个.gitattributes文件，添加如下内容：
    ```
    build.css diff=nodiff
    build.js  diff=nodiff 
    ```
当我们运行git diff命令的时候，git就会读取.gitattributes的配置，然后发现遇到build.css和build.js文件的时候，需要执行nodiff指令。这里文件路径的配置和.gitignore是一样的，支持目录，文件，通配符之类的，可以轻松实现忽略一批文件或者整个目录。