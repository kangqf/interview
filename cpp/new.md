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

``` cpp
new T;      // 调用 operator new(sizeof(T))
            // (C++17) 或 operator new(sizeof(T), std::align_val_t(alignof(T))))
new T[5];   // 调用 operator new[](sizeof(T)*5 + overhead)
            // (C++17) 或 operator new(sizeof(T)*5+overhead, std::align_val_t(alignof(T))))
new(2,f) T; // 调用 operator new(sizeof(T), 2, f)
            // (C++17) 或 operator new(sizeof(T), std::align_val_t(alignof(T)), 2, f)
```

根据使用的 operator new 的位置的不同，这就有了第一个可选参数`::`的使用:

``` cpp
::new T; // 从全局作用域查找operator new函数
new T; // 从类T的作用于开始查找operator new函数
```

### operator new
这个new 是一个分配函数，对于 全局的 operator new 就是调用malloc分配内存然后返回一个指针。对于一个类而言，它可以自行设计自己的 operator new 来实现内存的管理，比如说，不使用 malloc进行实现，而通过栈实现来new出栈上的空间。

#### 重载 operator new
下面是一个重载 operator new 的例子：

```
class T{

public:

T(int i):member1(i){}
~T(){}
// 此处声明为static或non-static均可，下同
void *operator new(size_t size, const string& str) 
{
        cout << str << endl;
        return ::operator new(size);
}


private:
int member1;

};
```

我们这样就重载了一个 operator new 函数来实现分配函数调用的时候打印字符串，对于上面定义的 T 类型，我们可以使用：

``` cpp
auto p = new     ("kqf")         (T)        (123);
//           placement_params?   type     initializer
// 对应着 布置参数？ 类型 初始化器 其中123 作为形参直接初始化类
```

这里只是打印字符，更多的我们可以想到，在这里我们可以维护一个内存池来对我们的对象进行搞笑的内存管理。

#### 构造
分配完内存后我们便需要在内存上构造对象并初始化。例如下面的代码：

``` cpp
void *buf = // 在这里为buf分配内存
Class *pc = new (buf) Class();  // 这里构造并初始化对象
```

这里所 用到的new就是布置new，也就是用于构造对象的new

### placement new   布置new
placement new 的功能就是 在一个 已经分配好的空间上，调用构造函数，创建一个类，就像上面的代码那样。

#### `new operator` `operator new` `placement new` 区分 {#diffofthreenew}

```cpp
class A {
public:
A(int i):a(i);
~A();
void *operator new(size_t size, const string& str)  // 这里是operator new
{
        cout << str << endl;
        return ::operator new(size);
}
private:
int a;
};

A *p = new("kkk") A(123); // 这里是new operator

int buf[sizeof(A)];   // 在栈上，分配一个数组
A *obj =  new(buf) A(123);  // 这里是 placement new 这里的buf应该也是placement_params
```

上述代码描述了三个概念的区别，同时在栈上面创造了一个对象。其中`placement new`那一步可以在`operator new`中实现，这就可以使得使用`new operator`得到的对象是在栈上的。因此，我们一般说 new 出来的对象内存是在`自由存储区`的，而不是像 malloc 说的一定是在堆中的。

## `new` 与 `malloc`的区别  {#diffofnewandmalloc}

1. new 不需要指定内存块的大小  malloc 需要
2. new 返回指向指定类型的指针 malloc 返回指向void * 需要自己转换
3. new 分配失败抛出异常 malloc 只是返回NULL
4. new 可以进行重载，自定义其行为 malloc 不可以
5. new 分配内存在自由存储区 malloc 在堆

[参考](http://www.cnblogs.com/maluning/p/7944231.html)

#### 内存分区
* 在C++中，内存区分为5个区，分别是堆、栈、自由存储区、全局/静态存储区、常量存储区；
* 在C中，C内存区分为堆、栈、全局/静态存储区、常量存储区；
