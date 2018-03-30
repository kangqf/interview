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
算法关键思想：每个合数必有一个最小素因子，用这个最小因子筛掉合数，注意的一点就是合数一定是通过**最小质素因子**来筛选的，如果用其他的的因子来筛选那么会出现 同一个合数 可能会有很多的因子，所以会对同一个合数进行多次筛选，为了避免这些没有必要的重复筛选，我们要求只能使用最小的素因子来筛选合数

``` cpp
void cp(long num)
{
    unordered_map<int,bool> flag;
    vector<int> re;

    // 默认所有数都是素数，然后剔除里面的合数
    for(int i = 0; i <= num; ++i)
    {
        flag[i] = true;
    }

    for(int i = 2; i <= num; ++i)
    {
        if(flag[i])
            re.push_back(i);//保存素数
        //剔除合数
        for(auto it = re.begin(); it != re.end() && *it * i <= num; it++)
        {
            flag[*it*i] = false;//找到合数，为了保证这里的质因子是最小的质因子，所有有下面的判断
            if(i % *it == 0)// 重点，这里是说，只要找到了 i 对应的最小的质因子， 就不用找了，
                // 因为 例如 当到4的时候 4*3 进行一次 判决的话，那么后面使用 6*2 会重复，里面的 4*3 的4可以分解为 2*2 而 2 已经处理过得到 3*2 = 6
                // 也就是说如果已经有 i 质因子，那么就可以跳出，因为，该质因子会通过其他的迭代方式来得到该数据
                break;
        }
    }

    for(auto it:re)
    {
        cout<<it<<endl;
    }
}
```