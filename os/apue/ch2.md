### 第二章

#### Unix 标准

1. ISO C 
    ISO C 最先由 ANSI 制定，然后被ISO采纳为国际标准 ISO/IEC 9899:1990
    
2. POSIX
    POSIX(Portable Operator System Inerface) 最初是由 IEEE 制定的标准族。该标准的1988版经修改后递交ISO，也就是最终的 IEEE 1003.1-1990 被ISO采纳为国际标准 ISO/IEC 9945-1:1990，通常称该标准为 POSIX.1 

3. SUS(Single UNIX Specification)
    SUS 是 POSIX.1 标准的一个超集，它定义了一些附加接口从而扩展了POSIX.1 的功能。XSI 是 POSIX.1 的可选项部分，其定义了遵循 XSI 标准的实现必须支持POSIX.1 中的哪些可选部分。 UNIX 商标被 Open Group 所拥有，其使用SUS来定义一系列接口，一个系统要想被称为UNIX系统，其实现就必须支持SUS中定义的接口
    
所以，从 所属关系来说 ISO C < POSIX.1 < SUS(UNIX)

##### ISO C 标准定义 24 个分区及其对应的头文件

```
 -------------------------------------------
| 头文件         |         说明             |    
|================|========================= |
| <assert.h>     | 验证程序断言             |
| <complex.h>    | 复数算术运算支持         |
| <ctype.h>      | 字符分类和映射支持       |
| <errno.h>      | 错误码                   |
| <fenv.h>       | 浮点环境                 |
| <float.h>      | 浮点常量及特性           |
| <inttypes.h>   | 整型格式变换             |
| <iso646.h>     | 赋值、关系及一元操作符宏 |
| <limits.h>     | 实现的限制的常量         |
| <locale.h>     | 本地化类别及相关定义     |
| <math.h>       | 数学函数、类型声明及常量 |
| <setjmp.h>     | 非局部goto               |
| <signal.h>     | 信号                     |
| <stdarg.h>     | 可变长度参数表           |
| <stdbool.h>    | 布尔类型和值             |
| <stddef.h>     | 标准定义                 |
| <stdint.h>     | 整型                     |
| <stdio.h>      | 标准io库                 |
| <stdlib.h>     | 实用函数                 |
| <string.h>     | 字符串操作               |
| <tgmath.h>     | 通用类型数学宏           |
| <time.h>       | 时间和日期               |
| <wchar.h>      | 多字节和宽字符支持       |
| <wctype.h>     | 宽字符分类和映射支持     |                            
 ===========================================
```


##### POSIX 标准定义的头文件
* POSIX 标准必需的头文件

```
 --------------------------------------------
| 头文件             |  说明                |                                                                     
|====================|======================|
| <aio.h>            | 异步IO               |
| <cpio.h>           | cpio归档值           |
| <dirent.h>         | 目录项               |
| <dlfcn.h>          | 动态链接             |
| <fcntl.h>          | 文件控制             |
| <fnmatch.h>        | 文件名匹配类型       |
| <glob.h>           | 路径模式匹配与生成   |
| <grp.h>            | 组文件               |
| <iconv.h>          | 代码集变换工具       |
| <langinfo.h>       | 语言信息常量         |
| <monetary.h>       | 货币类型与函数       |
| <netdb.h>          | 网络数据库操作       |
| <nl_types.h>       | 消息类               |
| <poll.h>           | 投票函数             |
| <pthread.h>        | 线程                 |
| <pwd.h>            | 口令文件             |
| <regex.h>          | 正则表达式           |
| <sched.h>          | 执行调度             |
| <semaphore.h>      | 信号量               |
| <strings.h>        | 字符串操作           |
| <tar.h>            | tar 归档值           |
| <termios.h>        | 终端IO               |
| <unistd.h>         | 符号常量             |
| <wordexp.h>        | 字扩充类型           |
|--------------------|----------------------|
| <arpa/inet.h>      | 因特网定义           |
| <net/if.h>         | 套接字本地接口       |
| <netinet/in.h>     | 因特网地址族         |
| <netinet/tcp.h>    | 传输控制协议定义     |
|--------------------|----------------------|
| <sys/mman.h>       | 内存管理声明         |
| <sys/select.h>     | select 函数          |
| <sys/socket.h>     | 套接字接口           |
| <sys/stat.h>       | 文件状态             |
| <sys/sstatvfs.h>   | 文件系统信息         |
| <sys/times.h>      | 进程时间             |
| <sys/types.h>      | 基本系统数据类型     |
| <sys/un.h>         | unix 域套接字定义    |
| <sys/utsname.h>    | 系统名               |
| <sys/wait.h>       | 进程控制             |
 --------------------------------------------

```


#### 限制
UNIX 系统下的限制主要有以下三种限制：

1. 编译时限制，通过头文件的形式来进行限制，例如 短整形的最大值之类的
2. 与文件或目录无关的运行时限制，也就是该限制在运行时确定，但跟具体的文件系统无关，例如 每秒的始终滴答数之类的。此类限制通过 `sysconf()`来获得
3. 与文件或目录有关的运行时限制，该限制不仅仅时运行时确定的而且还和具体的文件系统有关，例如 文件的链接计数的最大值。此类限制则通过 `pathconf()`和 `fpathconf()` 函数来获得


##### ISO C 限制

```
 -----------------------------------------------------------------------------------------
|     名称      |         说明                       |   典型值(Linux 32 bit system)     |
|===============|====================================|===================================|
| CHAR_BIT      | char 的位数                        | 8                                 |
| CHAR_MAX      | char 的最大值                      | 127                               |
| CHAR_MIN      | char 的最小值                      | -128                              |
| SCHAR_MAX     | signed char 的最大值               | 127                               |
| SCHAR_MIN     | signed char 的最小值               | -128                              |
| UCHAR_MAX     | unsigned char 的最大值             | 255                               |
|---------------|------------------------------------|-----------------------------------|
| INT_MAX       | int 的最大值                       | 2147483647                        |
| INT_MIN       | int 的最小值                       | -214783648                        |
| UINT_MAX      | unsigned int 的最大值              | 4294967295                        |
|---------------|------------------------------------|-----------------------------------|
| SHRT_MAX      | short 的最大值                     | 32767                             |
| SHRT_MIN      | short 的最小值                     | -32768                            |
| USHRT_MAX     | unsigned short 的最大值            | 65536                             |
|---------------|------------------------------------|-----------------------------------|
| LONG_MAX      | long 的最大值                      | 2147483647                        |
| LONG_MIN      | long 的最小值                      | -2147483648                       |
| ULONG_MAX     | unsigned long 的最大值             | 4294967295                        |
|---------------|------------------------------------|-----------------------------------|
| LLONG_MAX     | long long 的最大值                 | 9...(19位)                        |
| LLONG_MIN     | long long 的最小值                 | -9...(19位)                       |
| ULLONG_MAX    | unsigned long long 的最大值        | 18...(20位)                       |      
| --------------|------------------------------------|-----------------------------------|
| MB_LEN_MAX    | 一个多字节字符常量中的最大字节数   |                                   |
 ---------------|-------------------------------------------------------------------------
                                                                                                     
```

打印 ISO C 限制的例子：

``` c
#include <limits.h> 
#include <stdio.h>

int main()
{
    printf("INT_MIN: %d", INT_MIN);
}
```

##### POSIX 限制

POSIX 包含了 ISO C 的限制，并定义了自己的限制，这些限制很多涉及到了操作系统的实现。包括下面几个部分：

1. 数值限制：`LONG_BIT` `SSIZE_MAX` `WORD_BIT`
2. 最大值： `_POSIX_CLOCKERS_MIN`
3. 25 个最小值
4. 运行时不变值

* 25个最小值
POSIX 定义了实现要求的最小的值，这些值时最小的要求，尽管它们的名字里面大多含有一个 MAX ，而真实的实现的最小值一般都会比规定的最小值要大，实际实现的名字一般都是去除 `_POSIX_` 后得到的名字。

例如 要得到exec函数的参数的长度的规定的最小值和实际实现的最小值可以实用下面的代码：

``` c
#include <limits.h>
#include <stdio.h>
#include <stdlib.h>

int main()
{
    printf("_POSIX_ARG_MAX %d\n",_POSIX_ARG_MAX);
    printf("ARG_MAX %d",ARG_MAX);
    
    exit(0);
}
```

##### 运行时限制

```
#include <unistd.h>
long sysconf(int name);
long pathconf(const char *path, int name);
long fpathconf(int fd, int name);
```
如果 name 时不合法的，则返回 -1 并且设置errno 为 EINVAL，对于部分那么函数也会 返回 -1 提示该限制时不确定的，但时不会改变 errno

重复上面的例子：

``` c

#include <unistd.h>
#include <limits.h>
#include <stdio.h>

int main()
{
    printf("ARG_MAX %ld\n",sysconf(ARG_MAX));    
    printf("ARG_MAX %ld",pathconf(".",ARG_MAX));    
}

```


##### 基本系统数据类型
头文件 `<sys/types.h>` 定义了某些与实现有关的数据类型，被称为基本系统数据类型。具体如下：

````
-------------------------------------------------
| 类型             |     说明                   |
|==================|============================|
| clock_t          | 时钟滴答计数器             |
| comp_t           | 压缩的时间滴答             |
| dev_t            | 设备号                     |
| fd_set           | 文件描述符集               |
| fpos_t           | 文件位置                   |
| gid_t            | 数值组id                   |
| ino_t            | i节点编号                  |
| mode_t           | 文件类型，文件创建模式     |      
| nlink_t          | 目录项链接计数             |
| off_t            | 文件长度和偏移量           |
| pid_t            | 进程ID 和 进程组 ID        |  
| pthread_t        | 线程 ID                    |
| ptrdiff_t        | 两个指针相减的结果         |     
| rlim_t           | 资源限制                   |
| sig_atomic_t     | 能原子访问的数据类型       |          
| sigset_t         | 信号集                     |
| size_t           | 对象的长度                 |
| ssize_t          | 返回字节计数的函数         |   
| time_t           | 日历时间的秒计数器         |  
| uid_t            | 数值用户ID                 |
| wchar_t          | 能表示所有不同的字符码     |
-------------------------------------------------
```

