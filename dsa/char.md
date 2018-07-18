## 字符串总结

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
求两个字符串的最长的公共子序列

典型的二阶dp，和编辑距离很像，只是初始化默认是零，然后不需要特意初始化边界值

涉及这种问题的变种是把子序列变成子串，可以像参考代码注释部分类似的修改，但是时间复杂度是`O(m*n)`,另外可以有一种快速算法`KMP`,时间复杂度为`O(m+n)`

参考代码：
``` cpp
void outputlcs(string &str, string &pattern, vector<vector<int>> &dp)
{
    int i = str.length();
    int j = pattern.length();
    vector<char> re;
    while(i > 0 && j > 0)
    {
        if(str[i-1] == pattern[j-1])
        {
            re.push_back(str[i-1]);
            i--;
            j--;
        }
        else if(dp[i][j] == dp[i][j-1])
        {
            --j;
        }
        else if(dp[i][j] == dp[i-1][j])
        {
            --i;
        }
    }
    reverse(re.begin(),re.end());
    for(auto c:re)
    {
        cout<<c<<" ";
    }
}

int lcs(string &str, string &pattern)
{
    int len1 = str.length();
    int len2 = pattern.length();

    vector<vector<int>> dp(len1+1,vector<int>(len2+1,0));

    for(int i = 1; i <= len1; ++i)
    {
        for(int j = 1; j <= len2; ++j)
        {
            if(str[i-1] == pattern[j-1])
            {
                dp[i][j] = dp[i-1][j-1] + 1;
            }
            else
            {
                dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
                // dp[i][j] = 0; //这里是对于子串的修改，并且最后需要遍历一下dp求到最长的子串
            }
        }
    }

    outputlcs(str,pattern,dp);
    return dp[len1][len2];
}
```

### 最长回文子串
一个字符串得子串中是回文子串，且长度最长是多少得问题。

核心思想：维持 目前最长回文的中心 id，边界 mx，使用 i 遍历整个子串，初始化i处的最长回文长度P[i]，然后通过i处往两边扩展来更新 P[i]。最主要的优化就是这里的**P[i]的初始化时可以根据以前的信息来更快的初始**， 也就是 `P[i] = mx > i ? min(mx - i, P[2*id-i]) : 1;`，这里用的思想就是对称点和边界点，取到 P[2*id-i] 就是对称点，取到 mx-i 就是边界点。

处理细节，这里涉及到字符串长度奇偶的问题，可以通过Manacher 算法来处理这个细节。

参考代码：
``` cpp
string getManacherStr(string &str)
{
    string tmpStr = "^";
    for(int i = 0; i < str.length(); ++i)
    {
        tmpStr += "#";
        tmpStr += str[i];
    }
    tmpStr += "#$";
    return tmpStr;
}

int lps(string &str)
{
    string tmpstr = getManacherStr(str);
    vector<int> P(tmpstr.length(),1); // 初始化为1，就是最少长度都是1
    int id = 0;
    int mx = 0;

    for(int i = 1; i < tmpstr.length(); ++i)
    {
        if(mx < i)
        {
            P[i] = 1;
        }
        else
        {
            P[i] = min(mx-i,P[2*id-i]);
        }
        while(tmpstr[i-P[i]] == tmpstr[i+P[i]])
        {
            P[i]++;
        }
        if(i+P[i] > mx)
        {
            id = i;
            mx = i+P[i];
        }
    }
    int max = 0;
    for(int i = 0; i < tmpstr.length(); ++i)
    {
        if(P[i] > max)
            max = P[i];
    }
    return max-1;
}
```
### KMP
KMP用于在一个text中快速查找是否存在pattern子串。KMP 的复杂度是 O(n+m) 其核心思想就是不需要在不匹配的时候每次都返回0处重新开始比较，而是使用next数组来保存每次应该回退的长度，该长度表示的是**前一个元素结尾**的模式串的最长前缀后缀。所以next数组的构建需要先求最长前缀后缀，然后进行移位。

具体参考代码：
``` cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

// next[i] 表示的是最长前缀后缀的长度
// j 时前缀的端点， i 表示当前的后缀的端点
vector<int> getNextArray(string &pattern)
{
    vector<int> next(pattern.length(),0);
    int i = 1, j = 0;
    next[0] = 0;
    while(i < pattern.length())
    {
        if(pattern[j] == pattern[i])
        {
            next[i] = j+1;
            i++;
            j++;
        }
        else
        {
            if(j == 0) //前缀回到端点
            {
                next[i++] = 0;
            }
            else
                j = next[j-1]; // 回到上一个层次的前缀的比较
        }
    }
    return next;
}

// 真正意义上的next数组就是最长前缀后缀往右移一位，然后赋初值-1
void moveNextArray(vector<int> &next)
{
    int i;
    for(i = next.size()-1; i > 0; --i)
    {
        next[i] = next[i-1];
    }
    next[i] = -1;
}

vector<int> kmp(string &text, string &pattern, vector<int> &next)
{
    int i = 0, j = 0;
    vector<int> re;
    int len1 = text.length(),len2 = pattern.length();
    while(i < len1 && j < len2)
    {
        if(next[j] == -1 || text[i] == pattern[j])
        {
            if(text[i] == pattern[j])
                j++;
            i++;
            if(j == len2)
            {
                re.push_back(i-j);
                j = j-next[j]-1;
            }
        }
        else
        {
            j = j-next[j]-1;
        }
    }
    return re;
}

int main()
{
    string text = "BABABABAABABA"; //index = 5;
    string pattern = "ABAABABA";
    vector<int> next = getNextArray(pattern);
    moveNextArray(next);
    vector<int> re = kmp(text,pattern,next);
    for(auto c:re)
    {
        cout<<"Find pattern at "<<c<<" 's char"<<endl;
    }
    return 0;
}
```

### AC 自动机