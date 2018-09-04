### C++ 标准库

#### string 
string 是一个模板typedef 因此不能继承，能够支持下面一些函数的使用。

``` cpp
empty(); 
size(); 
length();
string::size_type; // 无符号整数
decltype(str.size()) aaa; // 定义 size_type 变量: 
string str(num,ch); // 构造函数
for(auto &c:str) toupper(c); // 为了改变str中的字符的值，必须把循环变量变成引用类型
```
一些错误用法：
``` cpp
// string 的加法运算必须保证一边是string 不能全是字面值
string str = "hello" + "world"; // 错误把字面值赋给string
```
对于string中的每个字符，有以下函数可以使用：
``` cpp
isdigit(ch);
isalpha(ch);
isalnum(ch);
islower(ch);
isupper(ch);
tolower(ch); // for(auto &c:str) c = tolower(c);
toupper(ch);
```
#### c标准库的string函数
``` cpp
strlen(p);
strcmp(p1,p2);
strcat(p1,p2); // 把p2 附加到p1 后面
strcpy(p1,p2); // p2 复制到p1
string str;
str.c_str(); // 获取string对象的c风格字符串
```

#### vector
构造函数应该区分 () 与 {} 其中 () 是函数调用 {} 是初始化列表
``` cpp
vector<int> v1(10,0); // 10 个 0
vector<int> v2{10,0}; // 一个10 一个0
```
vector 支持的一些函数操作
``` cpp
push_back();
empty();
size();

```

#### bitset
bitset里面支持了很多与位有关的操作，bitset类是一个类模板，接收一个表示二进制位的个数的非类型模板参数
```
// 构造函数
bitset<n> b;// 每位初始化为0
bitset<n> b(ull);//将一个 unsigned long long 的类型的低n位拷贝到b 例如 bitset<32> b(0xff);
bitset<n> b(bitstr);//从一个string构造，string中只能包含 0 或 1
bitset<n> b(str,last); // 使用string 中最后last个字符
bitset<n> b<str,pos,num); // 从pos开始使用，使用num个字符

// 重要操作
b.to_ulong();
b.to_ullong();
b.to_string();
b.test(pos); b[pos]; //查看pos位的值是否为1
b.count(); // 统计为1的位数数目
b.size(); // 返回b总共包含的位数
b.set(); // 将所有位置1
b.set(pos,v); // 将pos处的位赋值为bool值v
b.reset(); // 将所有位置0
b.reset(pos); // 将pos处的位置0
b.flip(); // 将所有位取反
b.flip(pos); // 将pos 处位取反
os<<b; // 以字符01 的方式打印b中的字符
in>>b; // 从in输入读取字符，直到读取b.size()个字符，或是遇到非01的字符

// 测试操作
b.any(); // 是否存在1
b.all(); // 是否所有位都是1
b.none();// 是否没有一个1

```

#### 迭代器
``` cpp
vector<int>::iterator ite;
vector<int>::const_iterator cite;
vec.begin();
vec.cbegin();
vec.cend();
vec.end();
int arr[] = {1,2,3};
vetctor<int> vec(begin(arr),end(arr)); // 使用数组类型初始化vector
```

#### 标准库函数
为了使得指针的使用更为安全，标准库使用了 begin() 函数与 end() 函数。
``` cpp
int a[] = {1,2,3,4};
int *begin = begin(a); // 首指针
int *end = end(a); // 尾后指针 
do{//toto}
while(next_permutation(str.begin(),str.end()));
```
