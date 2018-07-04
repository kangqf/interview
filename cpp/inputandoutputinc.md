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

// 同上面一组函数，但是使用的是 一组va_list，使用的时候会改变参数列表中的参数
#include <stdarg.h> 

int vprintf(const char *format, va_list ap);
int vfprintf(FILE *stream, const char *format, va_list ap);
int vsprintf(char *str, const char *format, va_list ap);
int vsnprintf(char *str, size_t size, const char *format, va_list ap);
```

4. EOF

值为 -1 文件输入结束的标识符，常用来判断输入是否完成

5. C 可变参数函数

``` c
#include <stdio.h>
#include <stdarg.h>

int mulSum(int firstNum, int secondNum, ...)
{
    int suma = 0, sumb = 0;

    va_list args; // 定义一个 va_list 变量
    va_start(args,secondNum); // 初始化变量，secondNum应该是最后一个命名参数的参数名
    fputs("\nfirst args",stderr);
    // vfprintf(stderr," ARG: %d %d",args); //不能再此处是用该函数，因为此处会修改参数列表中的参数指针
    while(firstNum--)
    {
        suma += va_arg(args,int);
    }

    fputs("\nsecond args",stderr);
    // vfprintf(stderr," ARG: %d %d ",args); //同上
    while(secondNum--)
    {
        sumb += va_arg(args,int);
    }
    va_end(args);//释放刚刚初始化的变量

    return suma * sumb;
}

int main(int argc, char *argv[])
{
    printf("\nmulSum %d\n",mulSum(2,2,1,2,2,4));// (1+2)*(2+4) = 18 实现一个 求和相乘的函数，前两个表示求和的数字的个数分别是多少
}
```