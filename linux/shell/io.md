### shell 输入输出
#### 输出字符串

```
echo "It is a test"
等价于
echo It is a test
显示转义
echo "\"It is a test\""
```


* 输出命令执行的结果

```
echo `date`
```
* printf 类似于c的用法

```
printf "%d %s\n" 1 "abc"
%d %s %c %f  十进制整数  字符串   字符  浮点 
```
#### 输入变量

```
read name 

read -p "请输入一段文字:" -n 6 -t 5 -s password

-p 输入提示文字
-n 输入字符长度限制(达到6位，自动结束)
-t 输入限时
-s 隐藏输入内容
```


