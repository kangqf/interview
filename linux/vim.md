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

```
I 在行首
A 在行尾
O 在上一行
```

#### 删
`x` `X` `dd` `D` `u` `U` `ctrl+r` 


```
X 删除前面
D 删除到结尾
U 撤销所有操作
```

#### 查
`f` `F` `t` `T` `/` `?` `*` `#`


```
fx  移动到当前行向后查找到的第一个x
Fx  移动到当前行向前查找到的第一个x
t与f的区别在于t移动到字符位置之前，而f则直接移动到字符处
*   向后搜索当前光标处的内容，等价于按下/然后输入当前光标处的单词
#   向前搜索当前光标处的内容
```


插件[ctrlsf.vim](https://github.com/dyng/ctrlsf.vim)

依赖于ack 或 grep  或 ag 对于 msys2 需要先安装 `pacman -S mingw-w64-x86_64-ag`

然后设置选项`let g:ctrlsf_ackprg = 'ag'`
```
<Leader>sp :CtrlSF<CR>  整个工程内查找
p                       得到搜索结果预览后按p 查看上下文源码
q                       退出预览窗口
<Enter>                 跳到搜索到的位置
```

#### 改 替换
`r` `R`


vim 自带的替换

`:[range]s/{pattern}/{string}/[flags]`

```
:s/thee/the       替换当前行第一个 thee 为 the
:s/old/new/g      替换当前行所有的 old 为 new
:%s/old/new       替换当前文件的所有行的第一个 old 为 new
:%s/old/new/g     替换当前文件的所有行的 old 为 new
:%s/old/new/gc    替换当前文件的所有行的 lod 为 new 并且每次需要确认
:bufdo            对打开文件进行替换
:args **/.cpp **/.h 对工程内所有文件进行替换
```

封装的函数

```
<Leader>R    不确认、非整词 
<Leader>rw   不确认、整词    
<Leader>rc   确认、非整词
<Leader>rcw  确认、整词
<Leader>rwc  确认、整词
```

`cc` 或 `S`  替换整行，也就是删除当前整行并进入 insert 模式。

多光标操作，使用插件：[multiple-cursors 插件](https://github.com/terryma/vim-multiple-cursors)

```
Ctrl+n -> Shift+n 选中下一个 
Ctrl+p            选中上一个
Ctrl+x            跳过
```


使用多光标的方式一般是

`;sp` 查找到多行，然后 `vaw` 或 `viw` 可视选中一个单词，`Ctrl+n` 选中后面的，然后 `shift+i` 插入多个位置，按下 ESC 然后保存。

也可以 `vip` 选中一个段落，然后 `Ctrl+n` 选中行首，`i`在行首插入内容

也可以 `fx`找到第一个x 然后 `Ctrl+n` 选中多个x，然后 `shift+i` 插入新的内容 


#### 复制粘贴

```
"+y  <Leader>y   复制内容到系统剪贴板
"+p  <Leader>p   粘贴系统剪贴板到vim
gp               在当前行的下一行进行粘贴，等价于 p 然后移动到下一行
gP               在上一行粘贴 等价于 P 然后移动到下一行
```

#### 查看文件信息
`Ctrl+g` `g+Ctrl+g`  前者查看概要信息，后者查看详细的信息


#### 移动光标
`w` `b` `e` `ge` `$` `g_` `0` `^` 
 
`h` `j` `k` `l` `H` `L` `M`

`G` `gg` `5gg` `+` `-` `(` `)` `{` `}`  `Ctrl+u` `Ctrl+d` 



```
w 单词前，后一般跟i
e 单词后，后跟a
b 后跟 i
ge 后跟 a
``` 

```
H 前半段
L 后半段
M 中间段
```

```
+         == j
-         == k
Ctrl+u   上翻页
Ctrl+d   下翻页 一次半屏
Ctrl+f   下翻页 一次一屏    
( )      句子
{ }      段落
```


#### 操作tag
`Ctrl+i` `Ctrl+t` `Ctrl+o` `g]` `ctrl+]` 

```
Ctrl+i  再次进入tag
Ctrl+t  返回上个标签
Ctrl+o  返回光标上次停留行，不会更新 jump 历史
`       跳到上一个位置，会更新 jump 的历史
g]      罗列当前光标处的单词的所有候选标签列表
ctrl+]  将光标处的单词所匹配的所有标签压入标签栈中  
```

引入tag

```
ctags -R --c++-kinds=+p+l+x+c+d+e+f+g+m+n+s+t+u+v --fields=+liaS --extra=+q --language-force=c++
:set tags+=/data/workplace/example/tags
```


tag可视化插件 [tagbar](https://github.com/majutsushi/tagbar)

```
;tb                        打开关闭tag 的可视化
s                          切换tag的排序方式
<Leader>tn :tnext<CR>      下一个标签
<Leader>tp :tprevious<CR>  上一个标签
```

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

`<` `>` `=` `<<` `>>` `==` `za` `zA` `zM` `zR` `%`

```
za 打开或关闭当前折叠
zA 打开或关闭所有的折叠
zM 关闭所有折叠
zR 打开所有折叠
<Leader>M %
```

插件[Indent Guides](https://github.com/nathanaelkane/vim-indent-guides)

`;ig` 显示或关闭缩进


彩虹括号[rainbow](https://github.com/luochen1990/rainbow)

不仅仅是括号，包括各种匹配符号



#### 窗口移动

```
nw <C‐W><C‐W>

<Leader>lw <C‐W>l
<Leader>hw <C‐W>h
<Leader>kw <C‐W>k
<Leader>jw <C‐W>j
```

```
<C‐W>s     将当前窗口分割为水平两个
<C‐W>v     将当前窗口分割为垂直两个
<C‐W>q     关闭当前窗口
<C‐W>w     遍历窗口
<C‐W>t     移动到最左上角的窗口
<C‐W>b     移动到最右下角的窗口
<C‐W>p     移动到前一个访问的窗口
<C‐W>r     向右或向下方交换窗口 R则和它方向相反
<C‐W>x     交换同列或同行的窗口的位置
<C‐W>=     让所有窗口调整至相同尺寸（平均划分）
<C‐W>-     将当前窗口的高度减少一行
<C‐W>+     将当前窗口的高度增加一行
<C‐W><     将当前窗口的宽度减少
<C‐W>>     将当前窗口的宽度增加
<C‐W>|     将当前窗口的宽度调到最大
```

#### 注释

插件[NERD Commente](https://github.com/scrooloose/nerdcommenter)

```
;cc  注释当前行或选中部分
;cu  取消注释当前行或选中部分
```

#### CPP 相关

高亮插件[vim-cpp-enhanced-highlight](https://github.com/octol/vim-cpp-enhanced-highlight)

实现与定义快速切换[vim-fswitch](https://github.com/derekwyatt/vim-fswitch)

`<Leader>sw :FSHere<cr>`

#### 执行命令 与 宏
`gQ`    进入Ex模式
`:!`    执行命令 如 :!ls
`qa`    录制宏 q 退出录制
`@a`    释放宏

#### 其它骚操作

[NERDtree](https://github.com/scrooloose/nerdtree) 插件可以查看文件列表
```
o 在之前的窗口打开文件，文件夹，书签
go 打开所选的文件，文件夹，书签，但是光标停留在NERDTree
t 在新的tab打开文件，文件夹，书签
T 打开所选的文件，文件夹，书签，但是光标停留在NERDTree
i 以分屏方式打开，文件，文件夹，书签
gi 以分屏方式打开，文件，文件夹，书签，但是光标停留在NERDTree
s 水平分屏打开文件，
gs 水平分别打开文件，但是光标停留在NERDTree
O 打开所选的文件夹的所有嵌套文件夹
x 关闭当前节点的父节点
X 递归关闭所有当前节点，及其子节点
e 编辑当前目录
```

[MiniBufExplorer](https://github.com/fholgado/minibufexpl.vim) 可以把所有 buffer 罗列出来


`v -> select -> : -> w test`   把选中的内容复制到文件

`:r !ls`         把 ls 得到的数据添加到当前文件，可以用来把命令的结果写入到当前文件

`:w !sudo tee %` 不小心打开了一个需要 sudo 的文件，而且做了不少更改，同步更改

`<C-[>`          返回到 normal 模式，等于按下 esc。




