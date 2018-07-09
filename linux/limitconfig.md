## 系统限制配置

### ulimit 
ulimit是linux shell的内键命令，它具有一套参数集，用于对 shell进程 及其 子进程 进行 资源限制

使用ulimit进行修改，是 立即生效 的

ulimit只影响shell进程及其子进程，用户登出后失效

可以在profile中加入ulimit的设置，变相的做到永久生效

`ulimit -a`可以查看所有的设定值

```
-t: cpu time (seconds)              unlimited
-f: file size (blocks)              unlimited
-d: data seg size (kbytes)          unlimited
-s: stack size (kbytes)             8192
-c: core file size (blocks)         0
-m: resident set size (kbytes)      unlimited
-u: processes                       62579
-n: file descriptors                1024
-l: locked-in-memory size (kbytes)  16384
-v: address space (kbytes)          unlimited
-x: file locks                      unlimited
-i: pending signals                 62579
-q: bytes in POSIX msg queues       819200
-e: max nice                        0
-r: max rt priority                 0
-N 15:                              unlimited
```

ulimit 的选线也输出在上面，例如我们可以利用 `ulimit -u 30` 来限制每个用户最多能有30个进程，还有两个选项是

-H 设置硬件资源限制

-S 设置软件资源限制（查询默认显示的是软件限制，修改默认是同时改变两者）

### limits.conf
位置在 `/etc/security/limits.conf`，主要的配置选项有

```
- core - limits the core file size (KB)
- data - max data size (KB)
- fsize - maximum filesize (KB)
- memlock - max locked-in-memory address space (KB)
- nofile - max number of open files
- rss - max resident set size (KB)
- stack - max stack size (KB)
- cpu - max CPU time (MIN)
- nproc - max number of processes
- as - address space limit (KB)
- maxlogins - max number of logins for this user
- maxsyslogins - max number of logins on the system
- priority - the priority to run user process with
- locks - max number of file locks the user can hold
- sigpending - max number of pending signals
- msgqueue - max memory used by POSIX message queues (bytes)
- nice - max nice priority allowed to raise to values: [-20, 19]
- rtprio - max realtime priority
- chroot - change root to directory (Debian-specific)
```

里面的配置生效时间是永久的 


### sysctl
sysctl是一个允许改变正在运行中的Linux系统的接口，修改的是针对整个系统的内核参数。sysctl的修改是 立即 且 临时 的（重启后失效）。可以通过修改/etc/sysctl.conf配置文件，达到永久生效。

主要的配置选项有

``` 
  -a, --all            display all variables
  -b, --binary         print value without new line
  -e, --ignore         ignore unknown variables errors
  -N, --names          print variable names without values
  -n, --values         print only values of a variables
  -p, --load[=<file>]  read values from file
  -r, --pattern <expression>
                       select setting that match expression
  -q, --quiet          do not echo variable set
  -w, --write          enable writing a value to variable
  -d                   alias of -h
```

常用的永久的修改方式是：修改`/etc/sysctl.conf`然后使用`sysctl -p`从配置文件中加载内核参数，使其立即生效

sysctl 能配置很多限制，例如IPC的绝大部分大小的限制都可以在里面配置，常用的还有ip转发的开启等。

### proc 文件系统
Linux内核提供了一种通过/proc文件系统，在运行时访问内核内部数据结构、改变内核设置的机制。

proc文件系统是一个伪文件系统，它只存在内存当中，而不占用外存空间。它以文件系统的方式为访问系统内核数据的操作提供接口。

最初开发/proc文件系统是为了提供有关系统中进程的信息。但是由于这个文件系统非常有用，因此内核中的很多元素也开始使用它来报告信息，或启用动态运行时配置。

对/proc中内核文件的修改，针对的是 整个系统 的 内核参数 ，修改后 立即生效 ，但修改是 临时 的（重启后失效）。

/proc/sys下内核文件与配置文件sysctl.conf中变量的对应关系为：去掉前面部分/proc/sys，将文件名中的斜杠变为点。例如：

``` 
/proc/sys/net/ipv4/ip_forward -> net.ipv4.ip_forward
/proc/sys/kernel/hostname -> kernel.hostname
```

proc 里面还有些常用的信息是：
```
/proc/meminfo 内存信息
/proc/cpuinfo CPU信息
/proc/sys/fs/file-max 文件打开数
/proc/sys/fs/file-nr 整个系统目前使用的文件句柄数量
/proc/sys/vm/mmap_min_addr 共享内存的最小地址
```

proc中与进程相关的信息
```
/proc/N pid为N的进程信息
/proc/N/cmdline 进程启动命令
/proc/N/cwd 链接到进程当前工作目录
/proc/N/environ 进程环境变量列表
/proc/N/exe 链接到进程的执行命令文件
/proc/N/fd 包含进程相关的所有的文件描述符
/proc/N/maps 与进程相关的内存映射信息
/proc/N/mem 指代进程持有的内存，不可读
/proc/N/root 链接到进程的根目录
/proc/N/stat 进程的状态
/proc/N/statm 进程使用的内存的状态
/proc/N/status 进程状态信息，比stat/statm更具可读性
/proc/self 链接到当前正在运行的进程
```

参考[ulimit、limits.conf、sysctl和proc文件系统](https://www.jianshu.com/p/20a2dd80cbad)