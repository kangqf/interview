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
``` cpp
// fun("1","2","3","4");

template<typename T, typename ... Args>
void fun(T &t, const  Args& ... rest)
{
    cout<<sizeof...(Args)<<endl;
    cout<<sizeof...(rest)<<endl;
}
```

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

#### 继承
继承是一种层次关系，被继承的类是`基类`，继承得到的类是`派生类`。

`派生类`通过`派生类列表`来指明是从哪个基类派生而来。`派生类列表`的格式是：派生类后面一个冒号，然后紧跟逗号分隔的基类，每个基类可以有访问控制说明符，类似于下面的方式：
``` cpp
class kkk: public kk, public k {

}
```

通过继承，我们可以使用同一段代码来分别处理基类和派生类的对象，从而实现了多态。这种方式我们称之为`动态绑定`或`运行时绑定`。

我们可以通过使用基类的`指针`或是`引用`来调用`虚函数`从而实现`动态绑定`。

* 继承与访问控制
在分析访问控制说明符之前，我们先要分清两个概念，一个是 类的设计者，一个是 类的使用者。

对于 无继承关系的类来说，类的设计者拥有类的绝对权限，也就是说类的所有成员的权限。而类的使用者，也就是实例化后的对象拥有类设计者指定的权限。

而我们所迷惑的protected 指的就是基类中的 protected 成员对于 `基类的使用者` 同样也是 `派生类的设计者` 部分是可访问的，我们给这种 同时具有`派生类设计者`和`基类使用者`两种身份的用户定义一个新名字为`类的设计使用者`，而对于派生类的使用者是不可见的，比如派生类实例化的对象或是派生类的友元（友元只对当前类的成员有效，而对于继承而来的类是无效的）等。

当引入继承关系后，派生类其实就是基类的类使用者，例如我们有下面这样的类设计：
``` cpp

class A {
private:
    int a;
protected:
    int b;
public:
    int c;
};

class B : public A {
public:
    B()
    {
        b = 1;// B() 是 A 的使用者也是 B 的设计者 所以 b 对其可访问
    }
    int e;
private:
    int f;
};

class C : protected A {
public:
    C()
    {
        b = 1;
        c = 1;
    }
};

class D : private A {
public:
    D()
    {
        b = 1;
        c = 1;
    }
};

class E : public C {
public:
    E()
    {
        b = 1; //这里可以使用 C 中的 protected 的成员 包括 C从A继承的protected 成员 b 这里特别注意
        c = 1;
    }
};
class F : public D {
public:
    F()
    {
        // b = 1; 因为D从 A继承是 private 所以即使是在 类的设计使用者这里也不能访问 protected 的成员
        // c = 1; 同上
    }
};

int main(int argc, char *argv[])
{
    
    B classB;
    C classC;
    D classD;
    //classB.b = 1; // 这里的classB 是B 的使用者 所以b对其不可访问。
    
    
    classB.c = 1; // 可访问 因为是public 控制的继承
    //classC.c = 1; // 不可访问 因为是 protected 控制的访问继承 classC 是类的使用者
    //classD.c = 1; // 不可访问  因为是 private 控制的访问继承即使是 classD 是类的使用者
    
    E classE;
    // classE.b = 1; // 这里的 classE 是类的使用者，而不是设计使用者，因此不能访问 b
    //classE.c = 1; // 由于 C 继承 A 是 protected 所以说C 中继承 A 的部分包括A 的public 部分只能在 C类的设计使用者处可以使用
    // 比如上面 派生类E的构造函数中
    
    return 0;
}

```
可以看出基类前面的`访问控制符`是想说明这样的一个关系：对于一个类那部分可见成员（包括 基类继承得到 protected 成员），`类的设计使用者`应该有的权限，以及`类的使用者`该有的权限，例如 protected 说明 `类的可见成员`只对 `类的设计使用者`可见，而对`类的使用者`不可见。

* 静态类型 与 动态类型
静态类型总是在编译时就已知了，而动态类型要等到运行时才知道它的具体类型。

#### 虚函数







