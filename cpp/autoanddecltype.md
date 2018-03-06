## auto 与 decltype 

### 使用场景
* auto 需要把表达式的值赋值给变量，并不能清楚的知道表达式的类型 `auto kkk = 2 + 3;`
* decltype 仅仅需要知道表达式表示的数据类型，而不需要用该表达式的值来初始化变量 `decltype(2+3) kkk = 5;`

### auto 使用方法：
* 推断基本类型： `auto kkk = a + b;`
* 一条语句中含有多个变量： `auto kkk = 9, *kk = &kkk;` 但是多个变量的类型不一致就会引发错误：`auto kkk = 1, kk = 3.14;`
* 推断引用时会把引用对象的类型作为推断类型：`int i = 0, &k = i;` `auto kk = k` kk将会是int类型
* auto 推断const类型，顶层const会被忽略，底层const会被保留。

    ``` cpp 
    const int ci=i,&cr=i;
    auto a=ci;      //a为int（忽略顶层const)
    auto b=cr;      //b为int（忽略顶层const，cr是引用）
    auto c=&i;      //c为int *
    auto d=&ci;     //d是pointer to const int(&ci为底层const）
    ```

* 使用auto声明顶层const：`const auto f = ci`
* 使用auto声明尾置返回类型的函数
    实例：申明一个返回指向一个维度为10的int数组的指针
      * 原生的方式： `int (*function(int i))[10]`
      * 使用尾置返回： `auto function(int i) -> int (*)[10]`
      * 使用类型别名： `typedef int arrT[10]` or `using arrT = int[10]` then `arrT* function(int i)`
      * 使用`decltype`：首先`int arr[] = {1,2,3,4}` then `decltype(arr) *function(int i)` 记得decltype返回的是类类型，要想返回指针就需要加上* 修饰符

### decltype 使用方法
* `decltype(f()) sum=x;` sum的类型就是f的返回值的类型
* 处理顶层const`const int ci=0;` `decltype(ci) x=0;`   x的类型是const int，该点与auto不一样
* 在处理引用上(与auto 也不一样)：
    * decltype使用表达式的结果类型可以作为一条赋值语句的左值，那么decltype返回一个引用类型
        ``` cpp
        int i = 0, *p = &i;
        decltype(*p) c = i; // c为引用类型必须要初始化
        ```
    * 如果表达式本身就是引用类型，那么decltype返回对应类型的引用类型
    ``` cpp 
    const int &cj=ci;
    decltype(cj) y=x;   //y的类型是const int&，y绑定到x上
    ```

[参考](http://blog.csdn.net/qq_14982047/article/details/50628075)