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
2. ` std::fstream s(filename, s.binary | s.trunc | s.in | s.out);` 文件操作
``` cpp
app	seek to the end of stream before each write
binary	open in binary mode
in	open for reading
out	open for writing
trunc	discard the contents of the stream when opening
ate	seek to the end of stream immediately after open

file.seekp(0,ios_base::beg);

beg	the beginning of a stream
end	the ending of a stream
cur	the current position of stream position indicator

```