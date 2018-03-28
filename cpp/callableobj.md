## 可调用对象
C++ 中的可调用对象主要包括：函数，函数指针，lambda表达式，bind创建的对象，以及重载了函数调用运算符的类实例化的对象。

`调用形式`用来区分不同的调用对象。`调用形式`主要包括了函数参数的类型和返回值得类型，例如：
`int(int,int)`。一种调用形式对应了一种函数类型，而两个不同类型的可调用对象可以共享同一种调用形式，例如：
``` cpp
// 普通函数
int add(int x, int y)
{
    return x+y;
}

// 函数指针
int (*add)(int x, int y)
{
    return x+y;
}

// lambda 表达式
auto add = [](int x, int y) -> int
{
    return x+y;
}

// bind 创建的对象
auto addone=bind(add, _1, 1);
// addone(1); 1+1=2

// 重载调用运算符的类的实例化可调用对象
class A {
public:
    int operator()(int x, int y)
    {
        return x+y;
    }
};

A a;
a(1+1);

```

### 函数表
为了统一管理上面所提到的可调用对象，我们希望建立一个函数表，但是通过vector来管理这些**不同类型的可调用对象**显然是不可行的，因此我们需要引入标准库的一个模板类 function 来管理相同`调用形式`但是 类型不同 的`可调用对象`。模板类的`实参`就是`调用形式`。例如我们可以通过下面的方式为我们刚刚刚创建的可调用对象创建一个函数表：
``` cpp 
function(int(int,int)) f1 = add; //上面的add 应该分别命名的 :(
map<string,function<int(int,int)>> functionSet;
functionSet["add"]=f1;
functionSet["+"]=f1;
```


### 函数对象
如果一个类重载了函数调用运算符，那么我们就能想使用函数一样使用该类的对象，这种类的对象我们称之为 `函数对象`。例如：

``` cpp
class A {
int operator()(int val) const
{
    return val < 0 ? -val : val;
}
};
// A 的实例化对象就是一个函数对象
// 我们可以使用下面这样的方式来调用
A a;
a(18);
```
函数对象很多被用在泛型算法中作为实参。

### 标准库的函数对象
标准库定义了一组很有用的模板类，这些类可以实例化函数对象，并将其用在泛型算法中。例如：
``` cpp
sort(ve.begin(),ve.end(),greater<int>());
sort(ve.begin(),ve.end(),less<int>());
```
我们也可以直接使用函数对象：
``` cpp
cout<<less<int>()(1,2)<<endl;
// 值得注意的是 less<int>(1,2) 的用法是错误的，因为less是个函数对象，也就是说要先构造对象，
// 然后初始化才能使用，这样相当于调用直接初始化的函数，而显然没有接收两个int 的构造函数，应此该直接初始化的方式是错误的
```

### lambda 表达式
lambda 从本质上说就是一个未命名的函数对象，并且重载了函数调用符。

对于值捕获的情况，函数对象会维护一个私有变量，而通过引用捕获的情况则不会维护私有变量，而是由程序自行确定变量是否存在。