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

// next[i] 表示的是以next[i]结尾的最长前缀后缀的长度
// j 是前缀的端点， i 表示当前的后缀的端点
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
            if(j == 0) //前缀回到端点 不用继续比了直接赋值0
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
            if(text[i] == pattern[j]) // 这个表示如果是从头开始的话 j就不递增
                j++;
            i++;
            if(j == len2) // 成功匹配
            {
                re.push_back(i-j);
                j = j-next[j]-1; // 这里也不用回退到开始的0位置
            }
        }
        else
        {
            j = j-next[j]-1; // 模式串往前移这么多
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
不同于KMP用于判断一个text串中是否包含另一个pattern串，AC自动机用于判断在一个text串中是否包含多个pattern串，或者说包含多个pattern串中的几个

利用的是字典树（Trie Tree）和KMP的思想，思想的原理还是要使得回退的次数最少，也就是常说的 **没有多余回溯的算法** 

对于这里的回退的节点，需要使用一个称为失配指针（fail 指针）的概念来指出。所谓的失配指针，其实和KMP里面的next数组是一样的含义，在KMP中next数组中记录的是前缀和后缀一样的最长的长度，而这里的fail记录的也是和当前节点后缀一样的最长前缀的节点的位置。如果当前节点匹配失败，就可以立即跳到该节点的位置来进行继续匹配下一个模式，fail指针的构建与next数组的构建也差不多，就是通过其父节点的fail指针回溯来找到fail指针该指向的位置。

``` cpp
#include <iostream>
#include <string>
#include <vector>
#include <queue>

using namespace std;

struct node
{
    int flag;// 判断是否是pattern的最后一个字符
    node *fail;
    node *next[26]; // 通过数组节点是否为空判断节点是否存在
    node()
    {
        flag = 0;
        fail=NULL;
        for(int i=0; i<26; i++)
            next[i] = NULL;
    }

};

void insertNode(node *root, string &str)
{
    node *tmp = root;
    for(int i = 0; i < str.length(); ++i)
    {
        if(tmp->next[str[i] - 'a'] == NULL) // 还没有添加该节点
        {
            tmp->next[str[i] - 'a'] = new node();
        }
        tmp = tmp->next[str[i] - 'a'];
    }
    tmp->flag += 1; // 表示这已经是一个pattern 的结尾了
}

node * constructTrieTree(vector<string> &pattern)
{
    node *root = new node();
    root->flag = -1; // 构建root节点
    for(int i = 0; i < pattern.size(); ++i)
    {
        insertNode(root,pattern[i]);
    }
    return root;
}

void setFailPoint(node *root)
{
    // 使用bfs + 队列来构建失效指针
    queue<node *> nodeQueue;
    nodeQueue.push(root);
    node *tmp;
    node *parFail;
    while(!nodeQueue.empty())
    {
        tmp = nodeQueue.front();
        nodeQueue.pop();
        for(int i = 0; i < 26; ++i)
        {
            if(tmp->next[i] != NULL)
            {
                if(tmp == root)
                {
                    tmp->next[i]->fail = root; // root 节点的所有fail指针都指向root
                }
                else
                {
                    parFail = tmp->fail;// 父节点的fail指针  注意root节点的fail指针为NULL
                    while(parFail != NULL) // 表示还没有回溯到root
                    {
                        if(parFail->next[i] != NULL) // 如果存在这样的相同前缀
                        {
                            tmp->next[i]->fail = parFail->next[i]; // 找到前缀与当前节点后缀相同的节点，则赋值fail指针
                            break;
                        }
                        parFail = parFail->fail;
                    }
                    if(parFail == NULL) // 没找到这样相同的前缀
                    {
                        tmp->next[i]->fail = root;
                    }
                }
                nodeQueue.push(tmp->next[i]); // 将子节点加入到队列中
            }
        }
    }

}

int acCount(string &text, node *root)
{
    int count = 0;
    node *curr = root;
    int ch;
    for(int i = 0; i < text.size(); ++i)
    {
        ch = text[i] - 'a';
        while(curr != root && NULL == curr->next[ch]) // 当前节点不为root 或者当前节点的下一节点与text[i]已经不匹配了，就通过fail找到匹配的
        {
            curr = curr->fail; // 找到匹配节点
        }
        curr = curr->next[ch]; // 将当前节点移到匹配的节点
        if(curr == NULL)
            curr = root; // 没有找到匹配节点
        node *tmp = curr; // 为了不改变当前的匹配节点所以新建一个变量
        while(tmp != NULL && tmp->flag != -1) // 没有回溯到root 并且没有被访问过
        {
            count += tmp->flag;
            tmp->flag = -1; // 改变其访问标识
            tmp = tmp->fail; // 通过fail继续进行回溯
        }
    }
    return count;
}

void del(node *root)
{
    if(root==NULL)return ;
    for(int i=0;i<26;i++)
        del(root->next[i]);
    delete(root);
}

int main(int argc, char *argv[])
{
    string text = "yasherhs";
    vector<string> pattern = {"she","he","say","shr","her"};

    node *root =  constructTrieTree(pattern);
    setFailPoint(root);
    cout<<acCount(text,root);
    del(root);
    return 0;
}
```
参考[AC自动机-算法详解](https://blog.csdn.net/u013371163/article/details/60469534)
[AC自动机模板](https://github.com/eecrazy/ACM/blob/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/AC%E8%87%AA%E5%8A%A8%E6%9C%BA/AC%E8%87%AA%E5%8A%A8%E6%9C%BA%E6%A8%A1%E6%9D%BF.cpp)

### 扩展学习
1. 无重叠最长重复子串 POJ 1743
2. 有关更多后缀数组的题 参考 vjudge->[后缀数组入门](https://vjudge.net/contest/58608#overview)(注 vjudge上面很多分类的题适合系统复习)
3. 可重叠的K次最长重复子串(POJ 3261)
4. 重复次数最多的连续重复子串的长度(SPOJ 687)
5. 求重复次数最多的子串 POJ 3693
6. 至少重复k次的可重叠的最长重复子串 POJ 3882
7. RMQ (Range Minimum/Maximum Query) 求解中的 ST 算法
8. LCA 与 RMQ
























