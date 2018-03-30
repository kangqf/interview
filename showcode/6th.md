### C/C++ A组 真题解答
1. 6届 6题
该题为dfs的典型例题
* 可以使用13个循环遍历后得到结果，优点是不需要脑子，不容易出错，缺点是代码有点多，要仔细编写。
* 也可以手写dfs，优点是代码量少，对循环的次数要求不高，缺点是需要脑子，针对一些边界条件要仔细。参考代码：
``` cpp
#include <iostream>

using namespace std;

// 将题转换成，一个连通图，每个节点都是一张牌的点数

int countCard = 0; // 累计有多少张牌
int countType = 0; // 累计有多少种牌型

// 形参是指的是 目前遍历的牌的点数
void dfs(int n)
{
    if(countCard > 13) // 累计的牌超过了界限，没有必要继续下去了
    {
        return ;
    }

    if(n == 14) //必须遍历完 13 种类型的牌，但是牌的数量可以是 0，必须遍历完，必须，必须
    {
        if(countCard == 13) // 集满13张牌
        {
            countType++;
        }
    }
    else
    {
        for(int i = 0; i < 5; ++i)
        {
            countCard += i;// 第n种牌 选择i张牌
            dfs(n+1);
            countCard -= i; //函数返回，则记了一次牌型，那么回溯一张牌
        }
    }
}


int main(int argc, char *argv[])
{

    dfs(1);
    cout<<countType<<endl;
    return 0;
}

```

2. 线性筛法求素数
算法关键思想：每个合数必有一个最小素因子，用这个因子筛掉合数。