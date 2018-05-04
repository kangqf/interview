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
`x` `X` `dd` `D` `u` `U`   

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

```

 搜索 word
模式：normal
按键：*
说明：光标在一个 word 中间时，按下星号 * 能向下搜索这个 word；之后再按 n 跳到下一个，N 跳到上一个匹配位置。
引申：井号 # 向上搜索这个 word


插件[ctrlsf.vim](https://github.com/dyng/ctrlsf.vim)

```
<Leader>sp :CtrlSF<CR>  整个工程内查找
p                       得到搜索结果预览后按p 查看上下文源码
q                       退出预览窗口
<Enter>                 跳到搜索到的位置
```

#### 改 替换
`r` `R`

<Leader>R 不确认、非整词:bufdo 对打开文件进行替换
<Leader>rw 不确认、整词:args **/.cpp **/.h 对工程内所有文件进行替换
<Leader>rc 确认、非整词
<Leader>rcw 确认、整词
<Leader>rwc 确认、整词

如果对打开文件进行替换，你需要先通过 :bufdo 命令显式告知 vim 范围，再执行替换；
如果对工程内所有文件进行替换，先 :args **/.cpp **/.h 告知 vim 范围，再执行替换；

:[range]s/{pattern}/{string}/[flags]
`:s/thee/the`
`:s/old/new/g`
`:%s/old/new/g`
`:%s/old/new/gc`



cc / S
说明： 替换整行，也就是删除当前整行并进入 insert 模式。

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
 
`h` `j` `k` `l` H L M
 
`G` `gg` `5gg` `+` `-` `(` `)` `{` `}`  `Ctrl+u` `Ctrl+d` 

<C-f> / <C-u>
说明：向下/向上翻页

#### 查看文件信息
`Ctrl+g` `g+Ctrl+g`  

#### 操作tag
`Ctrl+i`  `Ctrl+t` `g]` `ctrl+]`
`Ctrl+o`
<Leader>tn :tnext<CR>
<Leader>tp :tprevious<CR>


跳到上一位置
模式：normal
按键：`'
说明： 跳到上一个位置，会更新 jump 的历史，也就是说，多次使用该命令会在两个位置之间跳来跳去。
引申：<C-o> 也可以跳到上一个位置，不过它不会更新 jump 历史，会一直跳到文件关闭为止。




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

`sp` 将当前窗口分割为两个，当然每个窗口的 buffer 还是同一个文件


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
`gQ`

`p` `P` `gp` `gP`

 `:!`


`:help mode-switching`

#### 其它骚操作


NERDtree （https://github.com/scrooloose/nerdtree ）插件可以查看文件列表
MiniBufExplorer（https://github.com/fholgado/minibufexpl.vim 可以把所有 buffer 罗列出来


v -> select -> : -> w test

:r !ls  把 ls 得到的数据添加到当前文件

ea e到单词结尾进行添加
wi w到单词开头插入
bi b到单词开头插入
gea ge到单词尾进行添加


sudo 保存
模式：normal
按键：:w !sudo tee %
说明：不小心打开了一个需要 sudo 的文件，而且做了不少更改


<C-[>
说明：返回到 normal 模式，等于按下 esc。

```

U 撤销所有

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