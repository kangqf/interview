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
值得注意的是**无括号的 type 是贪心的：它将包含_ 任何能是声明器一部分 _的记号** 比如下面的用法：

``` cpp
int *p = new int + 1; // 用法正确 因为 + 不可能是声明器的一部分 等价于 int *p = new (int) + 1;

int *p = new int * 1; // 用法错误 因为 * 可以作为声明器的一部分 等价于 int *p = new (int *) 1;
```

另外，type 可以
