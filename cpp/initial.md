## 初始化 节

### 说明
对于一种初始化形式，我们说它属于哪种初始化方式，指的是它最表层看出来的初始化方式，也就是说它的类型可以变成其它类型，但是其初始化的方式还是不会变。

而对于给定的形式，可能该形式会根据类型的不一样（包括是否内置类型，构造函数的定义等）而属于其它的初始化方式，甚至，有的会执行两次初始化。但这些都不会影响最表层的初始化方式。

### 默认初始化 {#defaultInit}
这是在不使用初始化器构造变量时执行的初始化。

### 值初始化 {#valueInit}
变量以空初始化器构造时进行的初始化。

### 直接初始化 {#directInit}
以调用构造函数的形式显示初始化对象


```
Default initialization
Value initialization(C++03)
Copy initialization
Direct initialization
Aggregate initialization
List initialization(C++11)
Reference initialization
Static non-local initialization 
 zero - constant
Dynamic non-local initialization
 ordered - unordered
```