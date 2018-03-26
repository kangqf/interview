## 多态

### 编译时多态
编译时多态主要是通过 `函数重载` 和 `模板` 来实现的，模板包括 `函数模板` 和 `类模板`。这种多态又称之为 `泛型`。

#### 函数模板
``` cpp
template<typename T> 
int fun(T &t);

fun(8);
```

<typename T> 为一个模板参数，其中T为模板形参（只是定义了它但是并未说明如何初始化它），是一个 类 类型 的参数，称之为`模板类型参数`，与之对应的还有 不是 类 类型 的参数，称之为`非类型模板参数`，从8 推断出的类型int 是模板实参（隐式指定），也可以显示指定模板实参：`fun<int> (8);`。

* 普通函数模板
``` cpp
template<typename T>
int fun(T &t);
```

* 非类型模板参数
``` cpp
template<unsigned int N>
void fun(char (&p)[N]);
```
非类型参数可以是整型，也可以是对`对象` 或 `函数` 的 `引用` 或 `指针`，绑定到整型模板参数的实参必须是个常量表达式，绑定到引用或指针的实参必须静态生存期，也就是说我们不能把局部变量或动态对象用作指针或引用类型的实参。

*  `inline` 和 `constexpr`关键字 跟在 模板参数列表 之后
``` cpp
template<typename T> inline T &fun(T &t);
```

* 返回类型未知的尾置返回类型
``` cpp
template<typename T> ??? fun(T t>{return *t;}
// 我们不清楚*t 的具体类型，而且t是在参数之后才存在的，因此可以使用尾置返回类型
template<typename T> auto fun(T t) -> decltype(*t) {return *t;}
```
尾置返回类型的优点就是允许我们使用参数列表中的变量来推断返回类型，反之，不使用尾置返回类型的话因为形参的变量还未声明不能使用它作为返回类型

* 默认模板实参
``` cpp
template<typename T, typename M = int>
M fun(T &t);
```
默认模板实参既适用于函数模板也适用于类模板：
``` cpp
template<typename T = int>
class Object{};
```
我们可以适用 `Object<>`来使用默认地模板参数，也可以使用`Object<long>`来显式指定参数


#### 类模板
``` cpp
template<typename T> class Object{
private:
    T t;
}
``` 
向类模板传参必须要用显式传参，不像函数模板的隐式推断。例如`Object<int> obj;`来进行显式传参，其中 `int` 是 `显式模板实参`

* 类模板的模板类型别名
``` cpp
// 可以使用下面这种类型别名，但这种形式只适用于 实例化的模板版本
typedef Object<int> Obj;
// 我们不能使用下面这种方式
typedef Object<T> Obj;
//但是我们可以使用 using 声明来定义类型别名
template<typename T> using Obj = Object<T,T>;
```

* 类的 static 成员
对于普通类的static 成员，所有实例化的对象共享一个，而对于模板的static 成员是每个实例化的模板类都是共享一个

因此为了访问一个模板类的static 成员，必须使用实例化后地模板类加上域作用符来访问


* 使用模板参数的类型成员
假设有个模板参数 T，我们想使用 T 中的类型Type，我们会使用`T::Type`的形式，但是编译器会误认为Type 是T类的静态类型成员（默认情况下cpp会认为使用域作用符访问的名字是变量而不是类型）

为了消除这种误会，我们必须显式地告诉编译器该名字是一个类型而不是一个变量，这种情况需要使用 typename 来实现：`typename T::Type`

**这里是目前唯一一个typename 不能被class代替地地方**

* 控制实例化
为了控制模板在不同地文件内实例化同一个实例化的模板类，我们可以在模板不需要实例化的地方显式指明

使用 extern 声明标识该处不需要实例化模板，有点类似于类的前置声明
``` cpp
extern template<int> 
class Object{};
```

#### 可变参数模板
可变参数模板指的是模板的参数的数目是可变的，有点类似于 `initilalizer_list`

我们使用省略号来标识我们的参数是数目可变的参数
``` cpp
template <typename T, typename... Args>
void fun(T &t, const Args & ... rest);
```
第一行的 Args 是一个模板参数包，第二行的 rest 是函数参数包

对于 一个参数包我们可以使用 `sizeof...`来获其大小

* 包扩展
``` cpp
template<typename T, typename... Args>
T & fun(T &t, const  Args&... rest)
{
    cout<<rest...<<endl;
}
```
上面这种形式被称为包扩展，其中 第二行为`模板参数包`的`包扩展`，而第四行为`函数参数包`的`包扩展`

const & 修饰 Args 被称为 `扩展模式`

#### 模板特例化 与 函数重载
模板的特例化也可以看作是一种函数重载的方式，编译器会选择最特例化版本的那个函数使用。

### 运行时多态
运行时多态主要是通过 继承 和 虚函数 来实现的。










