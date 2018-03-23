### 华为编程题刷题笔记


1. 字符串最后一个单词的长度  
  
  处理输入就好
  
2. 计算字符个数
  
  处理输入以及字符串处理
  
  用到 *range for* `for(auto c : str)` 以及大小写的转换 `toupper(c)` `tolower(c)`

3. 明明的随机数  

  去重与排序
    
  用到 排序 `sort(data.begin(),data.end())`
  去重 `auto ite = unique(data.begin(),data.end())`
  ite为第一个重复元素的迭代器 `unique` 实际上并没有删除任何元素，而是将无重复的元素复制到序列的前端
  删除指定范围的元素 `data.erase(ite,data.end())`

4. 字符串分隔

  基本的字符串处理 注意边界条件

5. 进制转换

  十六进制转十进制 基本的字符串处理 注意处理循环多组数据的输入

6. 质数因子

  这里用到一些数学技巧，数学因子 是 将输入数值除以i(i从2开始)，除到无法整除(可以整除就输出然后做除法)后i++，直到除数平方大于等于输入数值
``` cpp
while(i*i < tmp)
{
    while(tmp1 % i == 0)
    {
        tmp1 /= i;
        cout<<i<<' ';
    }
    i++;
}
```

7. 取近似值

  将原数先乘以10 来分析其小数部分

8. 合并表记录
  
  使用 map 处理索引和值，并将相同索引的值相加

9. 提取不重复整数

  提取一个整数的每一位数，判断个位是否为0，然后逆序输出，输出时使用flag来去重。

10. 字符个数判断

  简单的字符串处理 也可以用set.insert() 来去重

11. 数字颠倒

  该题要求字符串的形式逆序输出， 因此要用到 `to_string(num)` 来将数字变为字符串

12. 字符串反转

  简单的字符串反转

13. 句子逆序

  简单的 vector 与 string 的联合使用

14. 字串的连接最长路径查找

  对n个字符串按照字典序排列 直接使用 vector<string> 加上 sort

15. 求int 型正整数在内存中存储时1的个数

  即求这个数转换成2进制后，输出1的个数 通过移位来解决
    ```cpp
    while(num > 0)
    {
        count += num & 0x01;
        num = num >> 1;
    }
    ```

16. 购物单

  典型的分组背包问题。具体参考 [背包问题](/)

17. 坐标移动

  主要是处理输入与输出
  
  判断是否是数字:`isdigit(str[i])`
  指定输入的分割符的方法：
  
  ``` cpp
  #include <sstream>

  string strt,str;

  while(cin >> strt)
  {
     istringstream iss(strt);
     while(getline(iss, str, ';')) // 以`;`分割来输入
  }
  ```
    将指定个数的数字字符转为数字
  ``` cpp
  string strTmp;
  strTmp.append(str,1,str.length()-1);
    step = stoi(strTmp);
```

18. 识别有效IP地址和掩码并进行分类统计

  字符串处理 主要用到 `atoi(str.c_str())`
  
  有个坑就是 有的ip 啥都不是， 比如 127.0.0.1/8
  还有一个坑就是 255.255.255.255 这个掩码是错误的
  实现了一个string 的 split
  ``` cpp
    vector<string> split(string &str, char sep)
    {
        stringstream sstr(str);
        vector<string> re;
        string tmp;
        while(getline(sstr,tmp,sep))
            re.push_back(tmp);
        return re;
    
    }
  ```

19. 简单错误记录

  字符串处理
  
  用到 `string.rfind()` 反向查找，返回第一个下标的位置
  `string.find()` 正向查找，返回第一个找到字符的下标
  还有一个有趣的结构体构造的代码：
``` cpp
struct ErrRecord{
    string file;
    int lineNo;
    int count;

    ErrRecord(string file, int lineNo){
        this->file = file;
        this->lineNo = lineNo;
        count = 1;
    }

    bool operator==(const ErrRecord & a){
        return (file == a.file) && (lineNo == a.lineNo);
    }
};
```
    然后利用 `ErrRecord record(getFileName(file), lineNo);`进行初始化

20. 密码验证合格程序

  字符串处理
  
  主要难点在 `不能有相同长度超2的子串重复` 这里涉及一个重复字串的处理 使用的是暴力穷举，但是能AC

21. 简单密码

  字符串处理
  
  简单的字符串处理，注意多值case语句的使用方法。以及 `Z` 后移一位是 `a`。

22. 汽水瓶

  简单的数值处理题
    
  ``` cpp
  while(cin>>num)
  {
      int count = 0;
      int remainder = 0;
      while(num/3 != 0)
      {
          count += num/3;
          remainder = num%3;
          num = num/3 + remainder;
      }
      if(num == 2)
          count += 1;
      cout<<count<<endl;
  
  }
  ```

23. 删除字符串中出现最少的字符

  简单的字符串处理，可以通过map统计出现次数后取最小次数的然后删除。
  
  注意 列表初始化： `int flag[3] = {0}` => [0 0 0] `int flag[3] = {1}` => [1 0 0]

24. 合唱队

  LIS 的变种 具体参考 [最长递增子序列-LIS](/)

25. 数据分类处理

  简单的字符串处理 貌似没有时间的要求 或者时间要求很宽裕，但是有多个case 输入的要求
  
  用到string的几个函数 `to_string( int ).find( string ) != string::npos`

26. 字符串排序

  简单的字符串处理

27. 查找兄弟单词

  简单字符串处理
  
  注意判断是否是兄弟单词的做法是使用排序后的字符串进行比较看是否相等，其次输出很坑的是换行，而且当k大于兄弟单词的总数时只输出兄弟单词的总数

28. 素数伴侣