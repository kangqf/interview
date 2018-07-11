## GDB 调试
```
gdb <program>                  调试程序
gdb <program> <core dump file> 调试coredump
gdb <program> <PID>            调试服务程序
```
### 常规操作
```
l      查看源码  list
b <n>  打断点 break
b <fun> 在fun函数名处打断点
r      运行到断点 run
c      下一个断点 continue
until <n> 运行到指定的行号
n      单步执行 next
s      单步跳入 step into
finish 跳出子函数
p <var> 查看变量值 print
q       退出
whatis <var>  查看变量类型，包含函数变量
watch <var>   监视某个变量，一旦其值发生变化就会输出提示
display <var> 每次程序暂停时都会输出该变量的值
bt/where      显示当前的堆栈列表 
info locals   显示当前堆栈页的所有变量
search <text> 在源代码中搜索文本
```

#### 断点操作
```
info b  查看所有的断点
info watchpoints 查看所有的监视点
enable <num>  使能断点号为num的断点
disable <num> 失能断点号为num的断点
delete <num>  删除断点号为num的断点
delete breakpoints 删除所有的断点
clear <line>    删除行号为line 的所有断点
```

#### 查看源码操作
```
l 输出当前位置后面的10行代码，并刷新当前位置
l <n>  查看第n行的前后5行代码
l <fname> 查看函数源码
```

#### 其他的骚操作
```
print fun(a) 将以变量 a(也可以自己指定变量) 作为参数调用 fun() 函数
call fun(a)  模拟调用函数fun
bt backtrace 显示当前调用堆栈
up/down      改变堆栈显示的深度
set args arg list    指定运行时的参数
show args    查看设置好的参数
set env var string     设置环境变量
show env var    查看环境变量
info program 来查看程序的是否在运行，**进程号**，被暂停的原因
info address <s> 查看符号s的地址
info frame   查看**堆栈寄存器**信息
x <address>  查看地址中的内容
```


### 多窗口操作
```
layout src   显示源代码
layout asm   显示汇编代码
layout reg   显示寄存器+src|asm(取决于那个最近被打开)
layout split 显示源代码和汇编窗口
layout next  下一个窗口
layout prev  上一个窗口
Ctrl + L     刷新窗口
Ctrl + x 1   单窗口模式，显示一个窗口
Ctrl + x 2   双窗口模式，显示两个窗口
Ctrl + x a   回到传统模式，即退出layout
```

在多窗口模式下 方向键操作的是layout里面的内容

### coredump 调试

Coredump叫做核心转储，它是进程运行时在突然崩溃的那一刻的一个内存快照。操作系统在程序发生异常而异常在进程内部又没有被捕获的情况下，会把进程此刻内存、寄存器状态、运行堆栈等信息转储保存在一个文件里。

#### 产生 core 文件
默认情况下不会产生core文件，因此要配置 `ulimit -c unlimited` 来临时让进程产生core文件，也可以通过`limits.conf`来永久配置，生成文件的文件名可以通过 `sysctl.conf` 或临时的 `/proc/sys/kernel/core_pattern` 的形式来修改。

#### 调试core文件
`gdb program core`来调试core文件，调试core文件可能会提示没有调试信息，这是因为编译的时候没有添加 `-g` 选项，但并不影响调试core文件，因为调试core文件的时候的符号信息主要来自符号表。不过加上调试信息后可以看到出错的代码的位置信息（行号）。

对于没有调试信息的core文件，首先通过`bt`来查看堆栈信息如下：
```
(gdb) bt
#0  __GI_raise (sig=sig@entry=6) at ../sysdeps/unix/sysv/linux/raise.c:51
#1  0x00007fd556501801 in __GI_abort () at abort.c:79
#2  0x00007fd55654a897 in __libc_message (action=action@entry=do_abort, fmt=fmt@entry=0x7fd556677b9a "%s\n") at ../sysdeps/posix/libc_fatal.c:181
#3  0x00007fd55655190a in malloc_printerr (str=str@entry=0x7fd5566797a8 "munmap_chunk(): invalid pointer") at malloc.c:5350
#4  0x00007fd556558ecc in munmap_chunk (p=0x55bc95c40744 <_fini>) at malloc.c:2846
#5  __GI___libc_free (mem=0x55bc95c40754) at malloc.c:3117
#6  0x000055bc95c406a9 in crashDump () at main.c:7
#7  0x000055bc95c406ba in main () at main.c:12
```

然后通过`f <n>`来跳到指定的堆栈，这里我们`f 6`跳到 `crashDump()` 函数所在的帧也就是第6帧

```
(gdb) f 6
#6  0x000055bc95c406a9 in crashDump () at main.c:7
7           free(ptr);
```

然后通过`disassemble`反汇编查看汇编代码

```
(gdb) disassemble
Dump of assembler code for function crashDump:
   0x000055bc95c4068a <+0>:     push   %rbp
   0x000055bc95c4068b <+1>:     mov    %rsp,%rbp
   0x000055bc95c4068e <+4>:     sub    $0x10,%rsp
   0x000055bc95c40692 <+8>:     lea    0xbb(%rip),%rax        # 0x55bc95c40754
   0x000055bc95c40699 <+15>:    mov    %rax,-0x8(%rbp)
   0x000055bc95c4069d <+19>:    mov    -0x8(%rbp),%rax
   0x000055bc95c406a1 <+23>:    mov    %rax,%rdi
   0x000055bc95c406a4 <+26>:    callq  0x55bc95c40550 <free@plt>
=> 0x000055bc95c406a9 <+31>:    nop
   0x000055bc95c406aa <+32>:    leaveq
   0x000055bc95c406ab <+33>:    retq
End of assembler dump.
```

箭头所指位置就是coredump时执行到的位置，通过`shell echo free@plt | c++filt`去掉函数的名词修饰，我们可以推测到这里是在调用free函数，我们就能知道我们coredump的位置，从而进一步能推断出coredump的原因。

#### coredump 产生的原因
1. 内存访问越界
 a) 由于使用错误的下标，导致数组访问越界
 b) 搜索字符串时，依靠字符串结束符来判断字符串是否结束，但是字符串没有正常的使用结束符
 c) 使用strcpy, strcat, sprintf, strcmp, strcasecmp等字符串操作函数，将目标字符串读/写爆。应该使用strncpy, strlcpy, strncat, strlcat, snprintf, strncmp, strncasecmp等函数防止读写越界。
2. 多线程程序使用了线程不安全的函数。
3. 多线程读写的数据未加锁保护。对于会被多个线程同时访问的全局数据，应该注意加锁保护，否则很容易造成core dump
4. 非法指针
 a) 使用空指针
 b) 随意使用指针转换。一个指向一段内存的指针，除非确定这段内存原先就分配为某种结构或类型，或者这种结构或类型的数组，否则不要将它转换为这种结构或类型的指针，而应该将这段内存拷贝到一个这种结构或类型中，再访问这个结构或类型。这是因为如果这段内存的开始地址不是按照这种结构或类型对齐的，那么访问它时就很容易因为bus error而core dump.
5. 堆栈溢出.不要使用大的局部变量（因为局部变量都分配在栈上），这样容易造成堆栈溢出，破坏系统的栈和堆结构，导致出现莫名其妙的错误。

参考
[gdb调试coredump(使用篇)](https://blog.csdn.net/u014403008/article/details/54174109)
[GDB调试core文件样例](https://blog.csdn.net/ithomer/article/details/5945152)

### 多线程调试
```
info threads 查看所有线程正在运行的指令信息
thread apply all <command> 让所有被调试的线程都执行command命令
thread apply <threadID> <command> 查看指定线程执行指定指令
thread <threadID>  进入指定线程栈空间 

```