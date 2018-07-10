### 其他常用的指令

#### find
参数：

```
-name 名称 -iname 忽略大小写的名称
-type f|d 类型，是目录还是文件
-mtime 修改日期 +表示之前 -表示之内  +1 一天前 -1 一天内 -atime access time 访问时间  -ctime 修改时间 把time改成min则以分钟为单位 例如 -mmin
-size 大小 +表示大于 -表示小于 
-perm 权限
-exec 执行命令 {} 传递找到的内容  \; 结束当前查找指令
-o 或 命令 等价于 |  用法是 find path \( -name "*.txt" -o -name "*.pdf" \) 查找两种文件类型 等价于  find path \( -name "*.txt" | -name "*.pdf" \) 
-regrex 正则匹配目录  find . -regex ".*\(\.py\|\.sh\)$"
-maxdepth 1 设置最大递归的层数
-mindepth 2 设置最小的递归层数
-user username 指定文件所属的用户
```

实例：
```
find -name "*.txt" -type f -mtime -1 -perm 777 | xargs rm -rf {} \;
find -name "*.txt" -type f -mtime -1 -perm 777 -size -1k -exec mv -r {} /tmp/ \;
```

#### grep
参数:

```
-c 指的是匹配的行数
-d 查找目录
-E 等价于使用egrep的正则匹配规则
-v 反转查找
-r 递归查询
-n 显示行号
```

设定前后打印的内容
`grep -{{C|B|A}} 3 {{search_string}} {{path/to/file}}` [C]ontext around, [B]efore, or [A]fter each match

实例：
```
grep -v -n --color "root" /etc/passwd

ifconfig | grep -E '([0-9]{1,3}\.){3}([0-9]{1,3})' --color

grep "h_est" ./ -r
```


#### paste

#### cut

