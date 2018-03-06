## 底层const 与 顶层const

* 顶层const：自己本身就是一个const
* 底层const：指向的对象是一个const 的对象。

例如这样的用法：`const int *const p = &val`我们以 * 修饰符为界分成两边：`const int | * | const p` 可以知道 p 是 const * （常量指针），指向 const int。 

#### const 与 整型

``` cpp
const int val = 10; // 这是一个顶层的const，val本身是一个常量
```

#### const 与 指针

``` cpp
const int *p = &val; // 这是一个底层const， p 是一个变量，该变量是一个指针类型，指向一个常量整型
int * const p = &val; // 这是一个顶层const，p本身就是一个常量的指针（常量指针），指向一个整型变量
const int * const p = &val; // 左边const 是底层const  右边 const 是顶层const
```

#### const 与 引用

``` cpp
int j=9;
const int &r2=j;    //底层const r2 被绑定到一个临时常量上，而不是真正的j上面 我们称之为对常量的引用
```

#### const 与 成员函数
在成员函数后面加上const表示该函数不会改变类的任何成员变量`const int fun(const int * const p) const{}`

### constexpr
constexpr 指定符声明可以在**编译时**求得函数或变量的值。 划重点，这里的constexpr限定了是编译期常量。
const 修饰的是一个变量，而constexpr修饰的是一个表达式。详见[cppreference例子](http://zh.cppreference.com/w/cpp/language/constexpr)

参考[这里](http://blog.csdn.net/qq_14982047/article/details/50615422)