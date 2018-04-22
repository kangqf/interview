### 第一章
#### `/etc/passwd` 文件
主要内容为：`sar:x:205:105:Stephen Rago:/home/sar:/bin/ksh`

从左到右以冒号分隔的各项分别是：用户名 加密后的用户密码 用户ID 用户组ID 注释 起始目录（家目录） 使用的shell

#### ls 实现
用到`dirent.h`头文件，以及头文件中的`readdir()`与`opendir()`两个函数和`struct dirent`结构体

另外还涉及一个`DIR`的结构体，这里暂时不提

代码如下：

``` c
#include "../apue/apue.h"
#include <dirent.h>

int main(int argc, char *argv[])
{
    DIR *dp;
    struct dirent *dirp;

    if(argc != 2)
        err_quit("Usage: kls dirname");
    if((dp = opendir(argv[1])) == NULL)
        err_sys("Can't open %s",argv[1]);
    while((dirp = readdir(dp)) != NULL)
        printf("%s\n",dirp->d_name);
    exit(0);
}
```

#### man 使用
man 用于查看函数或命令的手册，其格式如下：

`man num fun|command`

对于不同的num（章节号）的含义如下：

```
1   可执行程序或 shell 命令
2   系统调用(内核提供的函数)
3   库调用(程序库中的函数)
4   特殊文件(通常位于 /dev)
5   文件格式和规范，如 /etc/passwd
6   游戏
7   杂项(包括宏包和规范，如 man(7)，groff(7))
8   系统管理命令(通常只针对 root 用户)
9   内核例程 [非标准
```