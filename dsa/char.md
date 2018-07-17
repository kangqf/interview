## 字符串总结

LCS  LNRS  Edit Distance  KMP  Trie Tree  AC 自动机（hdoj 2222） 

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