### 关于对象

#### 封装的成本
对于一个C++类，函数是不占用空间的，只有数据成员会占用空间，一般只有有**两种情况**会给C++带来额外空间负担。
1. 虚函数指针 vfptr 用来支持“执行期绑定”（用基类指针调用子类函数）。
2. 虚基类指针 vbptr 用来实现共享虚基类的 subobject（多个子类共享一个基类）。

##### 内联函数的生成
对于 `函数的生成` 中文的翻译欠妥，原文如下：
```
one copy only of each non-inline member function is generated. 

Each inline function has either zero or one definition of itself generated within each module in which it is used.
```

作者想突出的是，inline 与 non-inline 函数的生成的所用域的不一样，也就是说，inline 的作用于在于`模块`，而 non-inline 的作用域是全局。也就是说两个都不会占用对象的空间，而翻译出来的很容易让人造成误解：inline 会占用对象的空间。

模块的含义就是：包含其定义与实现文件的所有文件组成的集合。一般 inline 的实现和定义一起都放在头文件中，这里要把两者放在一起也是作用域的问题，如果只在头文件中放置声明，而需要在其他文件中调用的话，仅仅包含该函数定义的头文件是不行的，还需要包含其实现文件。

而原文中 0 或 1 次生成的原因是，内联函数是在编译时展开，而不会存在普通函数调用的返回地址入栈出栈等操作，因此如果仅仅是定义了内联函数，而在模块中没有调用，那么，生成内联函数就是无用的，因此编译器可能放弃生成该函数。为了验证该观点，我做了如下实验：

* 普通函数定义并调用
``` cpp
➜  ~  cat test.cpp
#include <iostream>
using namespace std;
void fun(){    cout<<"kangqingfei.cn"<<endl;  }
int main()
{
    fun();
    return 0;
}
➜  ~ g++ test.cpp -o main
➜  ~ ls -al main
-rwxrwxr-x 1 kqf kqf 9248 7月  25 11:24 main
```

* 内联函数定义并调用
``` cpp 
➜  ~ cat test.cpp
#include <iostream>
using namespace std;
inline void fun(){    cout<<"kangqingfei.cn"<<endl;  }
int main()
{
    fun();
    return 0;
}
➜  ~ g++ test.cpp -o main
➜  ~ ls -al main
-rwxrwxr-x 1 kqf kqf 9248 7月  25 11:30 main
```

* 普通函数定义不调用
``` cpp
➜  ~ cat test.cpp
#include <iostream>
using namespace std;
void fun(){    cout<<"kangqingfei.cn"<<endl;  }
int main()
{
    return 0;
}
➜  ~ g++ test.cpp -o main
➜  ~ ls -al main
-rwxrwxr-x 1 kqf kqf 9248 7月  25 11:17 main
```

* 内联函数定义不调用
``` cpp
➜  ~ cat test.cpp
#include <iostream>
using namespace std;
inline void fun(){    cout<<"kangqingfei.cn"<<endl;  }
int main()
{
    return 0;
}
➜  ~ g++ test.cpp -o main
➜  ~ ls -al main
-rwxrwxr-x 1 kqf kqf 8904 7月  25 11:18 main
```
我们可以看到同样函数定义并调用，内联和普通函数生成的程序空间是一样大的，同样的函数定义了却没有调用，其中普通函数生成的程序占用空间要大些，而内联函数要小些，所以可以推测内联函数没有生成。
