## binutils 工具

### readelf
使用 readelf 显示一个或者多个elf格式的目标文件的信息

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

`readelf -l main` 查看区到段的映射，基本上是按照区的顺序进行映射，如果Flags为R和E，表示该段可读和可执行，如果Flags为W，表示该段可写。 

VirtAddr是每个段的虚拟起始地址，段有多种类型，例如LOAD类型 
LOAD：该段的内容从可执行文件中获取。Offset标识内核从文件读取的位置。FileSiz标识读取多少字节。

执行mian之后的进程的段布局可以通过cat /proc/[pid]/maps来查看。pid是进程的pid
但是该test运行时间很短，可以使用gdb加断点（b main在main函数打断点 然后r跑起来 然后info program 查看pid）来运行，或者在return语句之前加上sleep()


### objdump

### nm

### ar

### ld




参考： 
[readelf命令和ELF文件详解](https://blog.csdn.net/Linux_ever/article/details/78210089)
[程序或-内存区域分配（五个段）--终于搞明白了](https://blog.csdn.net/love_gaohz/article/details/41310597)