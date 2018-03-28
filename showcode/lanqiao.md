# 蓝桥杯
话说过两天就要参加第九届的初赛了，结果还没碰过这个鬼东西，为了不要太丢人，这两天打算刷刷往年的真题。

## 比赛相关
* 时间：4 小时 240 分钟
* 题量：10 题 初赛
* 题目类型与分值 
  * 填空题
    `1. 3分` `2. 5分` `3. 9分` 
  * 代码填空题
    `4. 11 分` `5. 13 分`
  * 填空题
    `6. 15 分` `7. 19 分`
  * 编程题
    `8. 21 分` `9. 23 分` `10. 31 分`
总分 150 分

## 比赛的环境
Dev-C++ 5.4.0 使用的编译器是 MinGW GCC 4.7.2 32 bit 理论上应该支持C++ 11的新特性，但是实测并不支持，很伤心。

## 细节注意
1. 代码填空不需要加分号
2. main函数结束必须返回0
2.  文件操作
``` cpp
// 调用函数打开文件
void open( const char *filename,ios_base::openmode mode = ios_base::in|ios_base::out );
// 使用构造函数打开
fstream( const char* filename,std::ios_base::openmode mode = ios_base::in|ios_base::out );
//一个示例
std::fstream s(filename, s.binary | s.trunc | s.in | s.out);
// 可用的魔术
app	seek to the end of stream before each write
binary	open in binary mode
in	open for reading
out	open for writing
trunc	discard the contents of the stream when opening
ate	seek to the end of stream immediately after open
// 判断是否打开成功
bool is_open();
// 关闭文件
void close();
// 获取当前位置
pos_type tellp(); // 输出位置
pos_type tellg(); // 输入位置

// 移动位置
// 移动输出
basic_ostream& seekp( pos_type pos );	
basic_ostream& seekp( off_type off, std::ios_base::seekdir dir );
// 移动输入
basic_istream& seekg( pos_type pos );
basic_istream& seekg( off_type off, std::ios_base::seekdir dir);
// 例如
file.seekp(0,std::ios_base::beg);
// 位置参数
beg	the beginning of a stream
end	the ending of a stream
cur	the current position of stream position indicator

// 处理输入
basic_ostream& operator<<( short value ); // 流操作符
basic_istream& get( char_type& ch );
basic_ostream& write( const char_type* s, std::streamsize count ); // 输入一个块

// 处理输出
basic_istream& operator>>( short& value );
basic_ostream& put( char_type ch ); // 输入一个字符
basic_istream& getline( char_type* s, std::streamsize count );
basic_istream& getline( char_type* s, std::streamsize count, char_type delim );
getline( std::basic_istream<CharT,Traits>& input, std::basic_string<CharT,Traits,Allocator>& str,CharT delim );
basic_istream& read( char_type* s, std::streamsize count ); // 输出到s
```

3. 排列组合函数 `next_permutation`
``` cpp
template< class BidirIt >
bool next_permutation( BidirIt first, BidirIt last );
	
template< class BidirIt, class Compare >
bool next_permutation( BidirIt first, BidirIt last, Compare comp );

一个生成全排列的算法
string str;
cin >> str;
sort(str.begin(), str.end());
cout << str << endl;
while (next_permutation(str.begin(), str.end()))
{
    cout << str << endl;
}
// cout可以替换成puts以提高速率
```

4. `unique`函数
该函数只会返回最后那个唯一的迭代器，要想得到真正的长度，应该用得到的迭代器减去begin，如果要删除重复的字符就应该用`erase(it,lll.end())`
