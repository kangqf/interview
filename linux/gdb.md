## GDB 调试

### 常规操作
```
l      查看源码  list
b <n>  打断点 break
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
display <var> 每次回车都会输出该变量的值
bt/where      显示当前的堆栈列表 
info locals   显示当前堆栈页的所有变量
```

#### 断点操作
```
info b  查看所有的断点
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
set args     参数:指定运行时的参数
show args    查看设置好的参数
info program 来查看程序的是否在运行，进程号，被暂停的原因。
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

### 多线程调试

