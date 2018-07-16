### 例举实际遇到的 使用shell进行处理的 问题

#### 批量修改文件后缀名
遇到的问题是：`Linux Shell Scripting Cookbook`一书的代码中的很多shell文件的后缀名被命名成了txt，现在需要把它改为sh

代码如下：

``` shell
#!/bin/bash
for file in ./*
do
    if [ -d "$file" ]
    then
        echo $file
        cd "$file"
        
        for scr in ./*.txt
        do
            newname=`echo $scr | sed 's/txt/sh/g'`
            # echo $newname
            # echo $scr
            mv "$scr" "$newname"
        done
                
        cd ..
    fi
done
```

#### 去除文件后面的 `^M` 字符
在很多windows创建的文件在linux打开会在结尾处看到`^M`的字符，现在需要去除它。

这里有两种方式 `dos2unix` 以及 `sed`

``` shell
#!/bin/bash

for dir in ./*
do
    if [ -d "$dir" ]
    then
        echo $dir
        cd "$dir"
        
        for file in ./*.sh
        do
            # 可以使用简单的 dos2unix
            # dos2unix $file
            # 注意^M的输入方式为 Ctrl+v,Ctrl+m 这里的 ^M 必须自己手敲出来，因为这个是显示不出来的自然就不能通过粘贴复制了
            sed -i 's/^M$//g' $file
        done
        
        cd ..
    fi
done
```

#### 提取单行部分的信息 作为 标签信息附加在行后
应用场景是深度学习的训练的时候，图片一般是有一个很长的名字，包括 类别信息，时间序列信息等乱七八糟的信息，我们需要将其中的类别信息提取出来，然后放到该行的后面以便训练。

例如`0002_c2s1_000301_01.jpg `的行 要变成 `0002_c2s1_000301_01.jpg 0002`

这里用到了 `cut` `paste` 命令

参考代码

``` shell
cat input.txt | cut -c 1-4 > num.txt
paste -d " " input.txt num.txt > output.txt
```

#### 几道面试题
```
find . -name '*.txt' -type f -exec rm {} \; # 用shell命令查找当前目录下所有包含txt的文件，并且删除他们 
ps -e | cut -c 25- | sed '1d' > test.txt  # 用shell命令将当前所有运行的进程的名字保存到一个文件
```