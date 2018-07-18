## 字符串总结

LCS  LNRS  Edit Distance  KMP  Trie Tree  AC 自动机（hdoj 2222） 

### 最长无重复子串-LNRS(Longest No Repeating Substring)
指的是一个字符串所有子串中，子串无重复字母的最大的长度。

使用的方法是解围法，维护一个hash表，和两个指针，一个指针前进，直到遇到有重复字母位置，这时另一个指针前移解围。

参考代码：
``` cpp
int lnrs(string& str)
{
    int len = str.length();
    unordered_map<char,int> flag;
    int i = 0, j = 0;
    int max = 0;
    while(i < len && j < len)
    {
        if(flag[str[j]])
        {
            flag[str[i++]]--;
        }
        else
        {
            flag[str[j++]]++;
            if(j - i > max)
                max = j - i;
        }
    }
    return max;
}
```

### 可重叠最长重复子串-LRS(Longest Repeat Substring)
最长重复子串，指的是在一个字符串里面找到两个一模一样的子串，并且要求使得它的长度最长。而可重叠的意思就是最长的两个子串允许有公共重叠部分，例如`banana` 可以有两个`ana`的子串，他们有一个`a`的重叠部分。

思路：

1. 先产生后缀数组
2. 对后缀数组进行排序
3. 对后缀数组中的元素与其前后两个进行比较，得到最相似的长度的值
4. 遍历所有的最相似的值得到最大值

参考代码：

``` cpp
int getCommonLength(string &a, string &b)
{
    int i;
    for(i = 0; i < a.length() && i < b.length(); ++i)
    {
        if(a[i] != b[i])
        {
            break;
        }
    }
    return i;
}

int nrlrs(string & str)
{
    int len = str.length();
    if(len == 1)
        return 0;
    vector<string> suffixArr;
    for(int i = 0; i < len; ++i)
    {
       suffixArr.push_back(str.substr(i,len-i));
    }

    sort(suffixArr.begin(),suffixArr.end());

    vector<int> maxLen(len,0);

    maxLen[0] = getCommonLength(suffixArr[0],suffixArr[1]);
    maxLen[len-1] = getCommonLength(suffixArr[len-1], suffixArr[len-2]);
    for(int i = 1; i < len-1; ++i)
    {
        maxLen[i] = max(getCommonLength(suffixArr[i-1],suffixArr[i]),getCommonLength(suffixArr[i],suffixArr[i+1]));
    }

    int maxLength = 0;
    for(auto c:maxLen)
    {
        if(maxLength < c)
            maxLength = c;
    }
    return maxLength;
}
```

### 编辑距离 Edit Distance
编辑距离指的是一个字符串 通过增加，删除，替换 三种操作变成另外一个字符串所需要的最少的次数。

这是一个典型的二阶动态规划问题，将`dp[i][j]`表征为`str1.substr(0,i)` 与 `str2.substr(0,j)`的编辑距离，这其实就是原问题的一个子问题。

`dp[i][j]` 更新：如果 字符相同，则转换为上一个子问题，不相同，则取三种操作中的最小的作为更新值。

参考代码：
``` cpp
int editDistance(string &str1, string &str2)
{
    int len1 = str1.length();
    int len2 = str2.length();

    vector<vector<int>> dp(len1+1,vector<int>(len2+1,0));
    for(int i = 0; i <= len1; ++i)
    {
        dp[i][0] = i;
    }
    for(int j = 0; j <= len2; ++j)
    {
        dp[0][j] = j;
    }

    for(int i = 1; i <= len1; ++i)
    {
        for(int j = 1; j <= len2; ++j)
        {
            if(str1[i] == str2[j])
            {
                dp[i][j] = dp[i-1][j-1];
            }
            else
            {
                dp[i][j] = 1+min(min(dp[i][j-1],dp[i-1][j]),dp[i-1][j-1]);
            }
        }
    }

    return dp[len1][len2];
}
```

### LCS

### 最长回文子串

### KMP

### AC 自动机