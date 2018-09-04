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
-D    设置宏
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

步骤 先使用 `-c` 生成 `.o` 文件，然后 `ar rcs libtest.a libtest.o` 生成静态库文件

可以通过 `nm -s libtest.a` 来查看库文件中的符号，可以以查看 .o 文件中的符号


### 编译动态库
文件结构
```
.
├── include
│   └── libtest.h
├── libtest.c
├── main.c
└── Makefile
```

文件内容
```
/****** include/libtest.h ******/
#include <stdio.h>

void printHello();

/****** libtest.c ******/
#include "libtest.h"

void printHello()
{
    printf("hello kqf\n");
}
/****** main.c ******/
#include "include/libtest.h"

int main()
{
    printHello();
}
/****** Makefile ******/
main : libtest.so
        gcc main.c -o main -L. -ltest -Iinclude

libtest.so : libtest.o
        gcc -shared libtest.o -o libtest.so

libtest.o : libtest.c include/libtest.h
        gcc -c -fPIC libtest.c -o libtest.o -Iinclude

run :
        export LD_LIBRARY_PATH=./:${LD_LIBRARY_PATH} && ./main

clean :
        rm *.so *.o main
```

步骤 使用 `-fPIC` 生成与位置无关的 `.o` 文件，使用 `gcc -shared libtest.o -o libtest.so` 生成动态库文件，导入动态库的目录到环境变量再运行。

`ldd libtest.so` 查看动态库所依赖的其他动态库。
