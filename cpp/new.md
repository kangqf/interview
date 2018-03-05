## 介绍 `new operator` `operator new` `placement new` 以及 `new`与`malloc` 的区别

### new表达式(new operator)
[new表达式](http://zh.cppreference.com/w/cpp/language/new)就是我们日常用到的new。new表达式可以分解为**分配**和**构造** 
* 分配：分配一个存储空间。
* 构造：在存储空间上初始化一个无名对象或数组，然后然后指向对应的对象的地址。

#### 基本语法为：

`::(可选) new (placement_params)(可选) ( type ) initializer(可选)`

#### 我们先省略其中的可选项可以得到下面的形式：

`new ( type )`

#### 或许我们还可以省略type的括号变成：

`new type`

这便是new 的最简单的形式了，例如我们可以这样使用：

`int *p = new int;`

这里，p指向的对象将会执行[默认初始化](./initial.md#defaultInit)，即 *p 的值是不确定的。

值得注意的是**无括号的 type 是贪心的：它将包含_ 任何能是声明器一部分  _的记号** 比如下面的用法：

``` cpp
int *p = new int + 1; // 用法正确 因为 + 不可能是声明器的一部分 等价于 int *p = new (int) + 1;

int *p = new int * 1; // 用法错误 因为 * 可以作为声明器的一部分 等价于 int *p = new (int *) 1;
```


另外，type 可以 使用 [`auto`与`decltype`](./autoanddecltype.md) 来推导出变量的类型。当使用这种方式时 initializer（初始化器） 不是可选的：因为要求使用它推导出替代 auto 的类型。所以可以有这样的用法：

`auto p = new auto('c');`

若type是数组类型，则其第一维必须是可以转换层size_t 类型的表达式或变量，例如下面的用法

``` cpp
int n = 42;
double a[n][5]; // 错误
auto p1 = new double[n][5]; // okay
auto p2 = new double[5][n]; // 错误
```

#### 现在我们加上初始化器，于是会有以下几种使用方式：

``` cpp
int *p = new int(); // 1
int *p = new int{}; // 2
int *p = new int(2); // 3
int *p = new int{2}; // 4
```

其中以上int 均可改成 Object。

* 对于方式1
    * 在C++03之前是执行[默认初始化](./initial.md#defaultInit)即其值不确定
    * 在C++03以后便是执行[值初始化](./initial.md#valueInit)
* 对于方式2，执行的是[值初始化](./initial.md#valueInit)
* 对于方式3与4，执行的是[直接初始化](./initial.md#directInit)

#### 分配
new 表达式通过调用适当的分配函数分配存储。若 type 是非数组类型，则函数名是 operator new 。若 type 是数组类型，则函数名是 operator new[] 。

在调用分配函数时， new 表达式将请求的字节数作为类型的 std::size_t 第一参数传递给它，该参数对于非数组 T 准确为 sizeof(T)

这就有了第一个可选参数`::`的使用:

``` cpp
::new T; // 从全局作用域查找operator new函数
new T; // 从类T的作用于开始查找operator new函数
```

### operator new
这个new 是一个分配函数，对于 全局的 operator new 就是调用malloc分配内存然后返回一个指针。对于一个类而言，它可以自行设计自己的 operator new 来实现内存的管理，比如说，不使用 malloc进行实现，而通过栈实现来new出栈上的空间。
