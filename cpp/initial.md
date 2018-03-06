## 初始化

### 说明
对于一种初始化形式，我们说它属于哪种初始化方式，指的是它最表层看出来的初始化方式，也就是说它的类型可以变成其它类型，但是其初始化的方式还是不会变。

而对于给定的形式，可能该形式会根据类型的不一样（包括是否内置类型，构造函数的定义等）而属于其它的初始化方式，甚至，有的会执行两次初始化。但这些都不会影响最表层的初始化方式。

### 默认初始化 {#defaultInit}
这是在不使用初始化器构造变量时执行的初始化。形式为：

``` cpp
T object ;
new T ;
```

在以下情况下发生默认初始化：
* 拥有`自动`、`静态`或`线程局域存储期`的变量声明不带初始化器时；
* 基类或非静态数据成员未在构造函数初始化器列表提及，且调用了该构造函数时。

默认初始化的方式为：
* 若 T 是数组类型，则每个数组元素都被默认初始化；

 ``` cpp
 T t[10];//对于每个t都是执行默认初始化
 ```

* 若T是类类型（如string或其他自定义类型），不管定义于何处，都会执行默认构造函数。如果该类没有默认构造函数（隐式产生的也算）比如构造函数为delete，则会引发错误。如果要定义const T 类类型的变量，则构造函数必须是由用户显式提供（不能是默认生成的）

 ``` cpp
 class T
 {
 public:
     T()=delete;
 private:
     int member1;
 };
 class A
 {
 private:
     int member1;
 };
 
 T t; // 错误用法，没有默认构造函数
 A a; // 正确用法，a执行默认初始化，调用隐式生成的默认构造函数，对于其成员`未在构造函数初始化器列表提及`也是执行默认初始化， 其值不确定
 const A a; // 错误用法，const 类型，用户没有显式提供构造函数
 ```

* 对于非类类型，如果是`静态` `线程局域(包括了全局)` 变量将先进行[零初始化](#zeroInit)，否则其值未确定。

 ``` cpp
 int x1; // 零初始化为 0 然后默认初始化不做任何事
 int main()
 {
    static int x2; // 零初始化为0
    int x3; // 未进行初始化，其值不确定
 }
 ```


### 值初始化 {#valueInit}
变量以空初始化器构造时进行的初始化。具体有以下几种形式：

``` cpp
T();	       
new T ();	
Class::Class(...) : member() { ... }	
T object {};	
new T {};	
Class::Class(...) : member{} { ... }	
```

值初始化方式为：
* 若 T 是数组类型，则数组的每个元素都被值初始化；
* 若 T 是 `无默认构造函数` 或 `拥有用户提供默认构造函数` 或 `删除的构造函数` 的类类型，则(默认初始化对象)[#defaultInit]
* 若 T 是 拥有默认构造函数的类类型，而默认构造函数既非用户提供亦非被删除（即它可以是拥有隐式定义或默认的默认构造函数），则先零初始化，然后若有非平凡的默认构造函数则默认初始化对象；
* 否则，对象被[零初始化](#zeroInit)


### 直接初始化 {#directInit}
以调用构造函数的形式显示初始化对象



### 零初始化 {#zeroInit}
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