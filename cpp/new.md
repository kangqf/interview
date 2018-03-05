## 介绍 `new operator` `operator new` `placement new` 以及 `new`与`malloc` 的区别

### new表达式(new operator)
[new表达式](http://zh.cppreference.com/w/cpp/language/new)就是我们日常用到的new。
基本语法为：

`::(可选) new (placement_params)(可选) ( type ) initializer(可选)`

我们先省略其中的可选项可以得到下面的形式：

`new ( type )`

或许我们还可以省略type的括号变成：

`new type`

这便是new 的最简单的形式了，例如我们可以这样使用：

`int *p = new int;`

这里，p指向的对象将会执行[默认初始化](#defaultInit)，即 *p 的值是未定义的。

值得注意的是**无括号的 type 是贪心的：它将包含_ 任何能是声明器一部分  _的记号** 比如下面的用法：

``` cpp
int *p = new int + 1; // 用法正确 因为 + 不可能是声明器的一部分 等价于 int *p = new (int) + 1;

int *p = new int * 1; // 用法错误 因为 * 可以作为声明器的一部分 等价于 int *p = new (int *) 1;
```

另外，type 可以 使用 [`auto`与`decltype`](./autoanddecltype.md) 来推导出变量的类型。当使用这种方式时 initializer（初始化器） 不是可选的：因为要求使用它推导出替代 auto 的类型。所以可以有这样的用法：

`auto p = new auto('c');`

现在我们加上初始化器，于是会有以下几种使用方式：
``` cpp
int *p = new int(); // 1
int *p = new int{}; // 2
int *p = new int(2); // 3
int *p = new int{2}; // 4
```

其中以上int 均可改成 Object。

* 对于方式1
    * 在C++03之前是执行[默认初始化](#defaultInit)即其值未定义
    * 在C++03以后便是执行[值初始化](#valueInit)
* 对于方式2，执行的是[值初始化](#valueInit)
* 对于方式3与4，执行的是[直接初始化](directInit)
