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

### vim 快捷键

#### 增
`i` `I` `a` `A` `o` `O` 

#### 删
`x` `X` `dd` `D`   

#### 查
`f` `F` `t` `T` `/` `?` `*` `#` `%` 

[ctrlsf.vim](https://github.com/dyng/ctrlsf.vim)
<Leader>sp :CtrlSF<CR>  search in project
p
q
<Enter>


#### 改
`r` `R`
`:s/thee/the`
`:s/old/new/g`
`:%s/old/new/g`
`:%s/old/new/gc`

多光标操作
[multiple-cursors 插件](https://github.com/terryma/vim-multiple-cursors)
查找到多行，然后vaw 或viw 可视选中一个单词，Ctrl+n 然后 I 插入


;rw 

#### 复制粘贴

"+y  <Leader>y

"+p  <Leader>p

#### 重做撤销
`u` `U` `ctrl+r` 

#### 移动光标
`w` `b` `e` `ge` `$` `g_` `0` `^` 
 
`h` `j` `k` `l`
 
`G` `gg` `5gg` `+` `-` `(` `)` `{` `}`  `Ctrl+u` `Ctrl+d` 

#### 查看文件信息
`Ctrl+g` `g+Ctrl+g`  

#### 操作tag
`Ctrl+i`  `Ctrl+t` `g]` `ctrl+]`
`Ctrl+o`
<Leader>tn :tnext<CR>
<Leader>tp :tprevious<CR>

`g]`      罗列当前光标处的单词的所有候选标签列表
`ctrl+]`  将光标处的单词所匹配的所有标签压入标签栈中
`;tn`     下一个标签
`;tp`     上一个标签

ctags -R --c++-kinds=+p+l+x+c+d+e+f+g+m+n+s+t+u+v --fields=+liaS --extra=+q --language-force=c++
:set tags+=/data/workplace/example/tags

nnoremap <leader>jc :YcmCompleter GoToDeclaration<CR>

nnoremap <leader>jd :YcmCompleter GoToDefinition<CR>


tag可视化插件
[tagbar](https://github.com/majutsushi/tagbar)
;tb
s





#### 书签
`'m` 引用书签。 例如：`g'm`  跳到`mark==m`处

* 独立书签

书签名只能由字母（a-zA-Z）组成，长度最多不超过 2 个字母

* 分类书签

分类书签，书签名只能由可打印特殊字符（!@#$%^&*()）组成，长度只能有 1 个字符

##### 书签可视化插件
[vim-signature](https://github.com/kshenoy/vim-signature)

`;mt` 开关书签可视化

* 独立书签

增删 `mx` `m,` `m.` `m-` `m<Space>` 遍历 `]反引` `[反引` 查 `;ms` `m/`

```
mx       在最左侧放置'x'书签
m,       放置下一个可用的书签
m.       如果当前行没有书签就反之下一个可用的书签，如果有就移除该书签
m-       删除当前行的所有书签
m<Space> 删除buffer 中的所有书签

]`       跳到下一个书签
[`       跳到上一个书签

m/       打开当前buffer 中的所有书签的列表
;ms      显示所有的书签列表，并且显示上下文内容
```


* 分类书签

增删 `m[0-9]`  `m<S-[0-9]>`  `m<BS>` 遍历 `]-`  `[-`  `]=` `[=` 查 `m?`

```
m[0-9]      放置分类书签
m<S-[0-9]>  移除一个类别的书签
m<BS>       移除所有书签

]-          跳到同类书签的下一行
[-          跳到同类书签的上一行
]=          跳到有分类书签下一行
[=          跳到有分类书签上一行

m?          打开当前buffer 中的所有分类书签的列表
```

#### 缩进与折叠


[Indent Guides](https://github.com/nathanaelkane/vim-indent-guides)

;ig

<
>
=
<<
>>
==


`za` `zA` `zM` `zR` 

<Leader>M %



彩虹括号，不仅仅是括号，包括各种匹配符号
https://github.com/luochen1990/rainbow



#### 窗口移动
nw <C‐W><C‐W>

<Leader>lw <C‐W>l
<Leader>hw <C‐W>h
<Leader>kw <C‐W>k
<Leader>jw <C‐W>j

#### CPP 相关
高亮插件
[vim-cpp-enhanced-highlight](https://github.com/octol/vim-cpp-enhanced-highlight)



实现与定义快速切换
[vim-fswitch](https://github.com/derekwyatt/vim-fswitch)
 <Leader>sw :FSHere<cr>


#### 执行命令 与 宏
`gQ`

`p` `P` `gp` `gP`

 `:!`


`:help mode-switching`

#### 其它骚操作



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