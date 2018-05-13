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

#### 输入和输出
对于系统函数的 标准输入是 0 标准输出是 1 但是更常用的是 `unistd.h`下的`STDIN_FILENO`和`STDOUT_FILENO`两个宏

对于库函数的 标注输入和输出是定义在 `stdio.h` 下面的 `stdin` 与 `stdout`

复制文件的程序：

非缓冲版本代码：

```
#include "../apue/apue.h"

#define BUFFER_SIZE 4096

int main()
{
    int n;
    char buf[BUFFER_SIZE];
    while((n = read(STDIN_FILENO,buf,BUFFER_SIZE)) > 0)
    {
        if(write(STDOUT_FILENO,buf,BUFFER_SIZE) != n)
            err_sys("write error");   
    }
    if(n < 0)
        err_sys("read error");

    exit(0);
}
```

缓冲版本:

```
#include "../apue/apue.h"

int main()
{
    int c;

    while((c = getc(stdin) != EOF))
    {
        if(putc(c,stdout) == EOF)
            err_sys("write error");
    }
    if(ferror(stdin))
        err_sys("input error");

    exit(0);
}

```

主要用到的函数为：`read()` `write()` `getc()` `putc()`

#### 进程控制
`getpid()` 返回进程的ID号，类型为`pid_t` 最大为长整形 `ld`

`waitpid()` 等待子进程退出，一般出现在父进程中

`fgets()` 一次读取一行，以换行为界限，当遇到结束符`Ctrl+d`时返回NULL。相比于`gets()` `fgets()`要求指定读取的最大的长度，这样可以防止读取一个缺失了结束标识符的流

`execpl()` 执行一个新的程序，要求参数以null结束而不是换行

`fork()` 创建一个新的子进程，一次调用两次返回，对于子进程返回 0， 对于父进程返回子进程的pid。

对于一个进程内的所有线程会共享同一个 地址空间 文件描述符 栈 以及进程相关属性，因此需要使用锁等机制来实现同步。

线程也用ID标识，但是线程ID只在它所属的进程内起作用。

参考代码：

```
#include "../apue/apue.h"
#include <sys/wait.h>

int main()
{
    char buf[MAXLINE];
    pid_t pid;
    int status;

    printf("%%");
    while(fgets(buf,MAXLINE,stdin) != NULL)
    {
        if(buf[strlen(buf) - 1] == '\n')
            buf[strlen(buf) - 1] = 0;
        
        if((pid = fork()) < 0)
            err_sys("fork error");
        else if(pid == 0)//child
        {
            execlp(buf,buf,(char *) 0);
            err_ret("could exec %s",buf);
            exit(127);
        }

        // parent
        if((pid = waitpid(pid,&status,0) ) < 0)
        {
            err_sys("wait error"); 
        }
        printf("%%");
    }  
    exit(0);
}
```

#### 出错处理
出错处理函数以及所有的错误码的定义包含在头文件 <errno.h> 中，其中 errno 作为标识错误类型的变量被实现为一个符号以避免线程之间的相互干扰。

`char * strerror(int errnum);` 将错误标识码 errnum 映射成为 字符串

 `void perror(char * msg);` 基于 errno 的当前值，把 `${msg}: ` 打印到标准错误输出  


#### 用户标识与用户组标识
定义在头文件 `unistd.h` 中的两个函数可以分别获取用户的id和用户的组id，其函数原型为：

`pid_t getuid();`

`uid_t getpid();`


#### 信号
信号的注册方式和回调函数的注册方式极为相似，我们可以使用下面的方式来注册我们自己的信号处理函数：

```
static void sig_int(int);

int signal(SIGINT,sig_int); // 注册 SIGINT 信号的自定义处理函数，如果注册失败会返回 SIG_ERR

void sig_int(int signo)
{
    dosomething();
}

```

#### 时间值

* 日历时间 

time_t 包含在头文件 <time.h> 中，包含了从19701.1 到现在的秒数

* 进程时间

使用 clock_t 保存这种时间值，用于表征进程从开始占用CPU到终止所花费的时间，又被称为 CPU时间 ，包括下面三个进程时间值：

1. 时钟时间，是进程运行的总时间量，与系统中进程的数目有关
2. 用户CPU时间，记录执行用户指令所占用的CPU的时间
3. 系统CPU时间，记录执行内核指令所占用的CPU的时间




