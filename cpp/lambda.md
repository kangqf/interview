## lambda 表达式
Lambda 表达式，也叫 Lambda 函数，也就是cpp中的匿名函数。 其实就是一个匿名函数 然后返回函数的调用地址。
### 基本语法
`[ captures ] <tparams>(可选)(C++20) ( params ) specifiers(可选) exception attr -> ret requires(可选)(C++20) { body }`

例如`auto glambda = []<class T>(T a, auto&& b) { return a < b; };`

* captures 为捕获列表
* tparams 提供模板形参 用于 实参 确定数据类型
* params 参数列表 不允许默认参数 因此形参的个数和实参的个数一定是一样的。

    从C++14开始，lambda表达式支持泛型：其参数可以使用自动推断类型的功能，而不需要显示地声明具体类型。这就如同函数模板一样，参数要使用类型自动推断功能，只需要将其类型指定为auto，类型推断规则与函数模板一样。这里给出一个简单例子：

    ``` cpp
    auto add = [](auto x, auto y) { return x + y; };
    
    int x = add(2, 3);   // 5
    double y = add(2.5, 3.5);  // 6.0
    ```

* specifiers 捕获限定符 一般情况下lambda不会改变使用值捕获的变量的值，如果我们想改变这个值，那么我们必须在参数列表后面跟上关键词mutable
* exception 异常规定
* attr 指定运算符 () 的属性 可以是 `const` `auto & ` 之类的指定符
* ret requires 返回类型 若缺失，则由函数的 return 语句所隐含（或若函数不返回任何值则为 void ）


* const lambda 声明：
`[ captures ] ( params ) -> ret { body }`
* 省略返回值：
`[ captures ] ( params ) { body }`
* 省略参数列表
`[ captures ] { body }`

举几个例子

``` cpp
[](int x, int y) { return x + y; } // 隐式返回类型
[](int& x) { ++x; }   // 没有return语句 -> lambda 函数的返回类型是'void'
[]() { ++global_x; }  // 没有参数,仅访问某个全局变量
[]{ ++global_x; }     // 与上一个相同,省略了()
```

### Lambda 的捕获
在Lambda中是可以使用局部形参以及全局变量的，但是试图使用任何未捕获的外部变量都是错误的。
为此，我们可以使用捕获列表来捕获外部的局部变量并用于lambda中，形式为：

``` cpp
[]        //未定义变量.试图在Lambda内使用任何外部变量都是错误的.
[x, &y]   //x 按值捕获, y 按引用捕获 x y 都是外部的函数的局部变量名
[&]       //用到的任何外部变量都隐式按引用捕获
[=]       //用到的任何外部变量都隐式按值捕获
[&, x]    //x显式地按值捕获. 其它变量按引用捕获
[=, &z]   //z按引用捕获. 其它变量按值捕获
```

例子

``` cpp
std::vector<int> some_list;
int total = 0;
for (int i=0;i<5;++i) some_list.push_back(i);
std::for_each(begin(some_list), end(some_list), [&total](int x)
{
    total += x;
});
```