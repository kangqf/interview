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

-S 设置软件资源限制（默认显示的是软件限制）

### limits.conf
位置在 `/etc/security/limits.conf` 
