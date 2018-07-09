## GCC 使用

### 常用选项
```
-E   预处理
-S   汇编
-c   编译
-o   指定输出文件名
-O   优化等级
-g   添加调试符号
-I   设置包含头文件路径
-L   设置链接库路径
-l   设置链接库
-Wall 设置警告等级
-D    设置变量
-std  指定c标准的版本
```

### 编译静态库
文件结构：

```
.
├── libtest.c
├── libtest.h
├── main.c
└── Makefile
```

文件内容：

```
/***** libtest.h *****/
#include <stdio.h>

void printHello();

/***** libtest.c *****/
#include "libtest.h"

void printHello()
{
    printf("hello kqf\n");
}

/***** main.c *****/
#include "libtest.h"

int main()
{
    printHello();
}

/***** Makefile *****/
main : libtest.a
        gcc main.c -ltest -L. -o main

libtest.a : libtest.o
        ar rcs libtest.a libtest.o

libtest.o : libtest.c libtest.h
        gcc libtest.c -c
run :
        ./main
clean :
        rm *.a *.o main


```