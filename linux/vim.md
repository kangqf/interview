## Vim

### Vim 的模式使用

#### Commands

```
d - delete(cut)
c - change
y - yank(copy)
v - visually select
> - indent 
< - dedent
= - reformat
```

按两次 `Commands` 就是在当前行执行该操作，例如 `dd` `cc` `yy` `==` `>>` `<<`

使用 `Commands` 的例子：

```
dw  - 删除到下一单词的开始
dt, - 删除文字直到，出现为止
de  - 删除到这一单词的结束
d2e - 删除到第二个单词的结束 等价于 2de
dj  - 删除本行和下一行
dt) - 删除文字直到 ） 出现为止
d/rails - 删除直到后面的第一次出现rails
```

#### Motions

```
a - all
i - in
t - till
f - find forward
F - find backward
```

使用 `Motions` 的例子：

```
iw, aw - 一个单词内，一个单词 仅仅是包不包括周围空格的区别
ip, ap - 一个段落内，一个段落
is, as – 一个句子内，一个句子
i), ap - 一个匹配内，一个匹配
i', a' - 单引号内
i", a" - 双引号内
it, at - 一个tag内，或一个tag
y'a    - 在某一行用ma做了标记（mark）， 使用y'a命令来复制这一行
```

**a 包括了单词周围的空格， i 却没有**

#### 结合使用

使用 `{number}{command} {text object or motion}`

例如：

```
diw  delete in word

caw  change all word

yi(  yank all text inside parentheses
```

相关插件 [vim-easymotion](https://github.com/easymotion/vim-easymotion.git)

参考[https://blog.carbonfive.com/2011/10/17/vim-text-objects-the-definitive-guide/](https://blog.carbonfive.com/2011/10/17/vim-text-objects-the-definitive-guide/)

### vim 原生按键

#### 增
`i` `I` `a` `A` `o` `O` 

#### 删
`x` `X` `dd` `D`   

#### 查
`f` `F` `t` `T` `/` `?` `*` `#` `%` 

#### 改
`r` `R`
`:s/thee/the`
`:s/old/new/g`
`:%s/old/new/g`
`:%s/old/new/gc`

#### 复制粘贴

#### 重做撤销
`u` `U` `ctrl+r` 

#### 移动光标
`w` `b` `e` `ge` `$` `g_` `0` `^` 
 
`h` `j` `k` `l`
 
`G` `gg` `5gg` `+` `-` `(` `)` `{` `}`  `Ctrl+u` `Ctrl+d` 

#### 查看文件信息
`Ctrl+g` `g+Ctrl+g`  

#### 操作tag
`Ctrl+i`  `Ctrl+o` `Ctrl+t` `g]` `ctrl+]`

g] 罗列当前光标处的单词的所有候选标签列表
ctrl+] 将光标处的单词所匹配的所有标签压入标签栈中
;tn
;tp

#### 书签
'mark 引用书签， g'mark  跳到mark处




#### 执行命令 与 宏


`gQ`

`p` `P` `gp` `gP`

 `:!`


`:help mode-switching`





v -> select -> : -> w test

:r !ls  把 ls 得到的数据添加到当前文件

ea e到单词结尾进行添加
wi w到单词开头插入
bi b到单词开头插入
gea ge到单词尾进行添加





```
x 光标上 
X 光标前
U 撤销所有
D 光标到行尾
I 行头插入
A 行尾插入
o 上面 加行
O 下面加行
w 首 后
b 首 前
e 尾 后
ge 尾 前
0 首
^ 首非空
$ 尾 
g_ 尾非空
5G = 5gg
- 上一行第一个非空字符
+ 下一行第一个非空字符
( 后句子
) 前句子
{ 后段落
} 前段落
Ctrl+u  上翻页
Ctrl+d  下翻页
Ctrl+g  显示详细信息 x Ctrl+g  /  g Ctrl+g
gp 命令是在当前行的下一行进行粘贴，并将光标移动到新插入行的下一行的开头处
gq 进入Ex模式
f 
F 
t 
T

```