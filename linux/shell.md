## shell

### shell变量

* 定义变量
`var="value"`

注意：

1. 等号前后没有空格
2. 变量前面不用$

* 使用变量

使用一个定义过的变量，只要在变量名前面加美元符号即可

```
var="kkk"
echo "$varkkk"   # $varkkk
echo "${var}kkk" # kkkkkk      双引号会解析里面的变量
echo "$var"kkk   # kkkkkk
echo '${var}kkk' # ${var}kkk   单引号不会解析任何变量
```

### 执行指令

* 使用` \`\` `

```
for file in `ls /etc`
```

* 使用`$()`

```
for file in $(ls /etc)
```

### shell 变量

```
1. $?  上次返回值
2. $#  参数个数
3. $n  $0 $1 代表第n个参数
4. $*  单行显示所有的参数，"$*"两个参数之间不分割
5. $@  单行显示所有的参数，"$@"两个参数之间要分割
6. $$  脚本运行的当前进程的ID号
7. $!  后台运行的最后一个进程的ID号
```


### shell 字符串

* 定义字符串

```
var=abcd
var='abcd'
var="abcd"
```

* 拼接字符串

```
var1="1234"$var"4567"
var2="1234${var}4567"
```

* 获取字符串长度

```
${#var} 
```

* 提取子字符串

```
${string:1:4}
```

### shell 数组
* 定义数组

```
arr=(value0 value1 value2 value3)
```

* 读取数组

```
val=${arr[n]}
echo ${arr[@]}  # 读取数组的所有元素
```

* 读取数组长度

```
# 取得数组元素的个数
length=${#arr[@]}
# 或者
length=${#arr[*]}
# 取得数组单个元素的长度
lengthn=${#arr[n]}
```
* 遍历数组

```
for v in  ${arr[@]}
do 
	echo $v
done
```

### shell 算术运算

```
val=`expr 2 + 2`
val=`expr $a + $b`
val=`expr $a \* $b`  # 乘法
b=$a                 # 赋值
re=$[a+b]        # 注意等号两边不能有空格
let s=(2+3)*4
```

### shell 关系运算符 只支持数字

```
[ $a == $b ]      # 判断变量是否相等，注意变量前后一定要有空格
[ $a != $b ]      # 判断变量是否不相等
[ $a -eq $b ]     # 相等
[ $a -ne $b ]     # 不相等
[ $a -gt $b ]     # 左边的数是否大于右边
[ $a -lt $b ]     # 左边的数是否小于右边
[ $a -ge $b ]     # 左边的数是否大于等于右边
[ $a -le $b ]     # 左边的数是否小于等于右边
[ ! false ]       # 取反运算
[ $a -lt 20 -a $b -gt 100 ]      # 与运算
[[ $a -lt 100 && $b -gt 100 ]]   # 与运算
[ $a -lt 20 -o $b -gt 100 ]      # 或运算
[[ $a -lt 100 || $b -gt 100 ]]   # 或运算
```

### shell 字符串运算符

```
[ $a = $b ]      # 字符串是否相等
[ $a != $b ]     # 字符串是否不相等
[ -z $a ]        # 字符串长度是否为0
[ -n $a ]        # 字符串长度是否不为0
[ $a ]           # 字符串是否不为空
```

### 文件测试 flag

```
语法 [ -flag $file ] 
flag 包括：
-b 块设备
-c 字符设备文件
-d 目录
-f 普通文件
-g SGID 位
-k 粘着位(Sticky Bit)
-p 有名管道
-u SUID 位
-r 读
-w 写
-x 可执行
-s 空（文件大小是否大于0）
-e 文件（包括目录）是否存在
```

### shell 语句

* for

```
for var in range
do
	do something
done
```

```
for((i=1;i<=5;i++));
do
    echo "这是第 $i 次调用";
done;
```

* if

```
if [condition]
then
	do something
else
	do otherthing
fi
```

```
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

* while

``` 
while condition
do
    command
done
```

```
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done

# 同样的作用不过是数字
i=1  
while((i<=5))  
do  
    echo $i  
    let i++  
done
```

* case

```
case val in
pattern1)
    command1
    ;;
pattern12）
    command1
    ;;
esac
```