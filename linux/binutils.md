## binutils 工具

### readelf
使用 readelf 显示一个或者多个elf格式的目标文件的信息，可以通过 `readelf -a programname`来读取主要的信息

#### elf文件
ELF（Executable and Linking Format）是一个定义了目标文件内部信息如何组成和组织的文件格式。内核会根据这些信息加载可执行文件，内核根据这些信息可以知道从文件哪里获取代码，从哪里获取初始化数据，在哪里应该加载共享库，等信息。 

ELF文件有下面三种类型： 
1. 目标文件
`gcc -c test.c`得到的`test.o`就是目标文件，目标文件通过链接可生成可执行文件。 
静态库其实也算目标文件，静态库是通过`ar`命令将目标打包为`.a`文件。 
如：`ar crv libtest.a test.o`

2. 可执行文件 
`gcc -o test test.c`得到的`test`文件就是可执行的二进制文件。

3. 共享库 
`gcc test.c -fPIC -shared -o libtest.so` 得到的文件libtest.so就是共享库。

#### 通过头部区分文件类型
可以通过readelf来区分上面三种类型的ELF文件，每种类型文件的头部信息（readelf -h 读取头部信息）是不一样的。
 
```
readelf -h ./makefile/test ./dynamic/libtest.so ./static/libtest.a

File: ./makefile/test
  Type:                              EXEC (Executable file)

File: ./dynamic/libtest.so
  Type:                              DYN (Shared object file)

File: ./static/libtest.a(libtest.o)
  Type:                              REL (Relocatable file)
```

#### 分析头部内容
```
readelf -h main
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              EXEC (Executable file)
  Machine:                           Advanced Micro Devices X86-64
  Version:                           0x1
  Entry point address:               0x400af0
  Start of program headers:          64 (bytes into file)
  Start of section headers:          172112 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           56 (bytes)
  Number of program headers:         9
  Size of section headers:           64 (bytes)
  Number of section headers:         38
```
1. 根据`Class`、`Typ`e和`Machine`，可以知道该文件在`X86-64`位机器上生成的64位可执行文件
2. 根据`Entry point address`，可以知道当该程序启动时从虚拟地址`0x400af0`处开始运行。这个地址并不是`main`函数的地址，而是`_start`函数的地址，`_start`由链接器创建，`_start`是为了初始化程序。通过这个命令可以看到`_start`函数：`objdump -d -j .text main | grep _start -C 10`
3. 根据`Number of program headers`可知该程序有9个段
4. 根据`Number of section headers`可知该程序有38个区，这比我们常见的 `.bss` `.data` `.text` `.rodata` 这些个区多了很多区，通过`readelf -S test` 来查看区的内容

#### readelf 查看符号和段
`readelf -s main` 可以看到test文件中所有的符号，Value的值是符号的地址

Segment(-l 查看)实际上就是由section(-S 查看)组成的，将相应的一些section映射到一起就叫segment了,就是说segment是由0个或多个section组成的，实际上本质都是section

`readelf -l main` 查看区到段的映射，基本上是按照区的顺序进行映射，如果Flags为R和E，表示该段可读和可执行，如果Flags为W，表示该段可写

VirtAddr是每个段的虚拟起始地址，段有多种类型，例如LOAD类型 

LOAD：该段的内容从可执行文件中获取。Offset标识内核从文件读取的位置。FileSiz标识读取多少字节

执行`main`之后的进程的段布局可以通过`cat /proc/[pid]/maps`来查看。pid是进程的pid
但是该`main`运行时间很短，可以使用gdb加断点（`b main`在`main`函数打断点 然后`r`跑起来 然后`info program` 查看pid）来运行，或者在return语句之前加上sleep()

得到的结果如下：

```
sudo cat /proc/20635/maps
00400000-00401000 r-xp 00000000 08:04 10358161                           /home/kqf/test/testc
00600000-00601000 r--p 00000000 08:04 10358161                           /home/kqf/test/testc
00601000-00602000 rw-p 00001000 08:04 10358161                           /home/kqf/test/testc
7ffff7a0d000-7ffff7bcd000 r-xp 00000000 08:04 2098731                    /lib/x86_64-linux-gnu/libc-2.23.so
7ffff7bcd000-7ffff7dcd000 ---p 001c0000 08:04 2098731                    /lib/x86_64-linux-gnu/libc-2.23.so
7ffff7dcd000-7ffff7dd1000 r--p 001c0000 08:04 2098731                    /lib/x86_64-linux-gnu/libc-2.23.so
7ffff7dd1000-7ffff7dd3000 rw-p 001c4000 08:04 2098731                    /lib/x86_64-linux-gnu/libc-2.23.so
7ffff7dd3000-7ffff7dd7000 rw-p 00000000 00:00 0
7ffff7dd7000-7ffff7dfd000 r-xp 00000000 08:04 2098727                    /lib/x86_64-linux-gnu/ld-2.23.so
7ffff7fd1000-7ffff7fd4000 rw-p 00000000 00:00 0
7ffff7ff7000-7ffff7ffa000 r--p 00000000 00:00 0                          [vvar]
7ffff7ffa000-7ffff7ffc000 r-xp 00000000 00:00 0                          [vdso]
7ffff7ffc000-7ffff7ffd000 r--p 00025000 08:04 2098727                    /lib/x86_64-linux-gnu/ld-2.23.so
7ffff7ffd000-7ffff7ffe000 rw-p 00026000 08:04 2098727                    /lib/x86_64-linux-gnu/ld-2.23.so
7ffff7ffe000-7ffff7fff000 rw-p 00000000 00:00 0
7ffffffde000-7ffffffff000 rw-p 00000000 00:00 0                          [stack]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]
```
前面第一列是VMA的起始地址和结束地址，最后一列是该区域内容所属文件。 

#### 再谈内存区域分配
1. 一般情况下，一个可执行二进制程序在存储(没有调入到内存运行)时拥有3个部分，分别是代码段(text)、数据段(data)和BSS段。这3个部分一起组成了该可执行程序的文件
2. 而当程序被加载到内存单元时，则需要另外两个域：堆域和栈域
3. 在将应用程序加载到内存空间执行时，操作系统负责代码段、数据段和BSS段的加载，并将在内存中为这些段分配空间。栈亦由操作系统分配和管理，而不需要程序员显示地管理；堆段由程序员自己管理，即显示地申请和释放空间

### objdump
对于`readelf` 可以通过`-S`看到section信息，而不能看到里面的汇编代码，我们可以通过 `objdump`来反汇编

`objdump -d program` 反汇编程序中执行指令的section，-D 会反汇编所有的section

`objdump -d -j .text program` 反汇编程序的指定section
`objdump -f program` 显示程序的头部信息，PHT(Program Header Table)
`objdump -h program` 显示程序的Section Header信息，SHT(Section Header Table)
`objdump -S a.out` 将C源代码和反汇编出来的指令对照，记得要在编译的时候加上 `-g` 选项

另外 -m 还可以指定架构 例如 `-m i386`

### nm
查看符号表

使用 `nm` 显示二进制目标文件的符号表，包括符号地址、符号类型、符号名等

符号前面加上了一个表示符号类型的编码字符。常见的各种编码包括：A 表示绝对 (absolute)，这意味着不能将该值更改为其他的连接；B 表示 BSS 段中的符号；而 C 表示引用未初始化的数据的一般符号。

`nm a.out` 查看`a.out`文件的符号表

```
-g  表示只看全局函数
--demangle 输出C++符号
-a 列出所有的符号
-u 只列出未定义的符号
```

### ar



### ld




参考： 
[readelf命令和ELF文件详解](https://blog.csdn.net/Linux_ever/article/details/78210089)
[程序或-内存区域分配（五个段）--终于搞明白了](https://blog.csdn.net/love_gaohz/article/details/41310597)