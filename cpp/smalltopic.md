## C++ 语言细节

### 负数取余

假设 `y = ax + b` 其中y是除数，x是被除数，a是商，b是余数
记住一点就是：b 应该与 a 同号，也就是说，余数永远和被除数同号
    -5 = -1 * 3 + -2 
    -5 = 1 * -3 + -2

这样就能得到对应的商和余数了，不管被除数是正还是负

### 数组做参数退化成指针 求其大小
很明显，如果使用数组名作为形参，会退化为指针进行传值，如何解决这个问题。

例如下面的代码：
``` cpp
void arrFun(char a[])
{
    cout<<sizeof(a)<<endl;
}

void arrRefFun(auto &a)
{
    cout<<sizeof(a)<<endl;
}

template <unsigned N>
void arrTemplateFun(const char (&a)[N])
{
    cout<<N<<endl;
}

int main(int argc, char *argv[])
{

    char a[10] = "abcd";
    char *b = new char[10] ();

    arrFun(a);// 4
    arrRefFun(a);// 10
    arrTemplateFun("abcd");// 5
    arrTemplateFun(a);// 10

    arrFun(b);// 4
    arrRefFun(b);// 4
    //arrTemplateFun(b);// 错误用法，不能实例化模板

    return 0;
}
```

其中第一种方式就是使用的是数组名传参，可以看到输出的是指针的大小为4byte，数组的维度信息丢失了

第二种方式使用新标准的auto的方式，得到的类型就是实参的类型，也就是说，如果又两个 数组 char a[10] char b[20] 使用auto对两者进行类型推断得到的两种类型是不一样的，一个是 维度为10 的数组，一个是维度是20的数组

第三种方式是通过非类型模板的形式进行模板实例化，这种方式有限制，只有维度是字面值，可以推断出N 的大小的形式才能使用，不过对于解决这个问题这种限制可以忽略。也就是说我们不能使用一个 `普通局部变量` 或 `动态对象` 作为 `指针` 或 `引用`  的 `非类型模板参数` 的 `实参`。