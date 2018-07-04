## C输入与输出

1. printf

``` c
printf("%-3.2f",var); // 输出一个浮点数，最少有三位数，小数位有两位，右对齐，补空格
printf("%05.2f",var); // 输出一个浮点数，最少有五位数，小数位有两位，左对齐，补0
```

2. 输出输入方式的比较

``` c
char getc(FILE *stream);        // 从指定流获取一个字符
void putc(char c,FILE *stream); // 输出一个字符串到指定流

fgetc(FILE *stream);           // 与getc一样
fputc(char c,FILE *stream);    // 与putc一样

char getchar(void);  // 回显 需要按回车 输入一行只取第一个字符
void putchar(char ); // 输出一个char 到标准输出

char getch(void);  // 不回显 不需要按回车 不是标准库函数
char getche(void); // 回显 不需要按回车 不是标准库函数

scanf("%s",str); //输入一个字符串，以空格为分割，不能得到输入的空格 

gets(char *);    // 读入一个字符串以换行为结束
puts(char *);    // 输出一个字符串

char * fgets(char * str, int num, FILE *stream); // 从指定流读入最多num个字符，以换行结束 推荐使用
char * fputs(char * str, FILE * stream ); // 输出字符串到指定流
```

3. printf 系列
区别主要在： 输出到的位置是 标准输出 还是 流对象(f) 还是 缓冲数组(s) 以及是否有最大的大小限制(sn)
``` c
#include <stdio.h>

int printf(const char *format, ...);
int fprintf(FILE *stream, const char *format, ...);
int sprintf(char *str, const char *format, ...);
int snprintf(char *str, size_t size, const char *format, ...);

int printf_s(const char *restrict format, ...); //printf只会检查格式字符串是否为空(null)，而printf_s还会检查格式字符串是否合法。

#include <stdarg.h>

int vprintf(const char *format, va_list ap);
int vfprintf(FILE *stream, const char *format, va_list ap);
int vsprintf(char *str, const char *format, va_list ap);
int vsnprintf(char *str, size_t size, const char *format, va_list ap);
```