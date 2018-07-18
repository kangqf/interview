## 动态规划

### 最长递增子序列问题 LIS(Longest Increasing Subsequence Problem)
找到一个序列中，所有递增子序列中长度最长的子序列的长度。子序列的取值不要求在原序列中连续

思路：动态规划，`dp[i]` 表示以`arr[i]`结尾的递增子序列的最长的长度。有递推公式:`dp[i] = arr[i] > arr[j] ? dp[j] + 1 : 1 j<i`;

参考代码：
``` cpp
// LIS 的常规形式 n^2
int lis(vector<int> &data)
{
    vector<int> dp(data.size(),0);
    for(int i = 0; i < data.size(); ++i)
    {
        dp[i] = 1;
        for(int j = 0; j < i; ++j)
        {
            if(data[i] > data[j]) // 如果不允许相等的严格递增就是 > 否则就是 >=
            {
                dp[i] = max(dp[i],dp[j]+1);
            }
        }
    }
    int max = 1;
    for(auto c : dp)
    {
        if(c > max)
            max = c;
    }
    return max;
}
```

进阶思路：动态规划+二分查找 

其想法就是每次可以将每次得到的有序信息保存起来，用于下次比较。

dp 数组用于保存当前的最长递增的子序列的全部的数值，更新dp：如果当前遍历的数值比dp中最后的元素的值还要大，那么增加dp的长度，并且在末尾添加，否则就只是单纯的替换。

参考代码：
``` cpp
// LIS 的二分形式
int lisBinSearch(vector<int> &data)
{
    vector<int> dp(data.size(),0);
    int len = 1;// 注意这里是1，因为 lower_bound() 的区间是 [first,last)
    dp[len] = data[0];
    for(int i = 1; i < data.size(); ++i)
    {
        // 这里如果找不到比data[i] 大的数就会返回last下标，这时pos+1 才会 > len，否则都是都会返回小于等于len的值
        //如果允许相等情况的非严格单调，则是upper_bound()，这个函数不允许相等，因此相等时仍然会返回last，把相等的data[i] 加入到末尾
        int pos = lower_bound(dp.begin(),dp.begin()+len,data[i]) - dp.begin();
        dp[pos] = data[i];
        len = max(len,pos+1);
    }
    return len;
}
```

### MCSS

### 单调栈

### 单调队列


### 背包