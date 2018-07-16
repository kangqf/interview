## Makefile

### make 的选项
```
-f 为 make 指定 makefile 文件
-I 指定 make 查找makefile文件的目录
-n 只是显示命令而不执行，这个可以用来调试makefile
-i 使所有的命令忽略错误
-k 跳过出错的命令的规则，继续执行其他的规则
```


### 基本语法
```
target : dep
	command
```
target 依赖于 dep，如果dep有更新，那么target 就会被更新，command是编译时执行的指令。反斜杠 `\` 用来分割多个依赖文件，当target需要依赖多个文件，但是在一行又放不下的时候就会用到`\`来分割。`#` 用来在Makefile 里面添加注释。

### 变量
#### 在Makefile中使用变量
使用`objects = main.o lib1.o lib2.o lib3.o`的形式在Makefile 中定义变量，然后使用`$(objects)` 的形式在Makefile中使用变量。

```
var := kkk 使用kkk变量定义var kkk 必须事先定义过
var ?= kkk 如果var没有定义则用kkk定义var
var += kkk  为var添加kkk
```

#### 为 make 传递变量
我们可以使用 `make KEY=VAL`的形式来向make传递参数，然后在Makefile里面使用`gcc ${KEY} main.c`来引用参数的值。

#### 变量中使用通配符
```
objects = *.o 相当于宏展开
objects := $(wildcard *.o)
```
```
$@ 目标的集合，类似shell的数组
$< 所有依赖目标的集合
% 所有的.o文件
```
#### VPATH变量
指定 make 查找依赖文件和目标文件的目录
vpath 命令
vpath <pattern> <directories>
例如 `vpath %.h ../headers`

### 自动推倒
make看到一个[.o]文件，它就会自动的把[.c]文件加在依赖关系中，如果make找到一个whatever.o，那么whatever.c，就会是whatever.o的依赖文件。并且 cc -c whatever.c 也会被推导出来。

### 伪目标
`.PYONY : action`用来定义伪目标，例如经常定义 clean 为伪目标，因为它需要没有依赖的文件，对于伪目标它总是被执行。
```
.PHONY : clean
clean :
	-rm edit $(objects)
```
其中`rm`前面的`-`的含义是可能某些文件删除出错但是不要管继续执行。这里的clean不是一个文件，它只不过是一个动作名字，有点像C语言中的lable一样，其冒号后什么也没有，那么，make就不会自动去找文件的依赖性，也就不会自动执行其后所定义的命令。要执行其后的命令，就要在make命令后明显地指出这个lable的名字。

“伪目标”的取名不能和文件名重名，不然其就失去了“伪目标”的意义了。当然，为了避免和文件重名的这种情况，我们可以使用一个特殊的标记“.PHONY”来显示地指明一个目标是“伪目标”，向make说明，不管是否有这个文件，这个目标就是“伪目标”。只要有这个声明，不管是否有“clean”文件，要运行“clean”这个目标，只有“make clean”。

### 其它
#### 引用其它Makefile
`include <filename>` 可以引用其它路径的Makefile，例如`include foo.make *.mk $(bar)`
#### 嵌套make
在多个文件夹中都放有一个Makefile，我们需要嵌套执行make
cd subdir && $(MAKE)
#### 子shell
```
cd dir 
./main

cd dir; ./main
```

为了保持cd对后续命令的作用，应该用分号的形式。

#### 自动查找头文件
使用编译器查找依赖的头文件`cc -M main.c`

