## auto 与 decltype 

### 使用场景
* auto 需要把表达式的值赋值给变量，并不能清楚的知道表达式的类型 `auto kkk = 2 + 3;`
* decltype 仅仅需要知道表达式表示的数据类型，而不需要用该表达式的值来初始化变量 `decltype(2+3) kkk = 5;`

### auto 使用方法：
* 推断基本类型： `auto kkk = a + b;`
* 一条语句中含有多个变量： `auto kkk = 9, *kk = &kkk;` 但是多个变量的类型不一致就会引发错误：`auto kkk = 1, kk = 3.14;`
* 推断引用时会把引用对象的类型作为推断类型：`int i = 0, &k = i;` `auto kk = k` kk将会是int类型
* 



[参考](http://blog.csdn.net/qq_14982047/article/details/50628075)