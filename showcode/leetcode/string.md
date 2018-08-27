#### Longest Palindromic Substring {#5}
经典的回文字符串的题，首先通过manchester把字符串扩充，然后维持一个当前回文边界mx 一个当前回文中心id，通过mx 或`P[2*id-i]`来初始化P[i]
然后更新 id 和 mx，注意更新的边界条件是 P[i]+i > mx 这里会有两种情况 一个是当前的回文长度P[i]很长，超过mx，也有可能是i太大超过了mx。
``` cpp
class Solution {
public:
    string manchester(string &s)
    {
        string re;
        re += "^";
        re += "#";
        for(int i = 0; i < s.length(); ++i)
        {
            re += s[i];
            re += "#";
        }
        re += "$";
        return re;
    }
    string longestPalindrome(string s) {
        string re;
        if(s.length() == 0)
        {
            return re;
        }
        s = manchester(s);
        int id = 0;
        int mx = 0;
        vector<int> P(s.length(),1);
        for(int i = 1; i < s.length(); ++i)
        {
            if(mx >= i)
            {
                P[i] = min(mx-i,P[2*id-i]);
            }
            else
            {
                P[i] = 1;
            }
            while(s[i-P[i]] == s[i+P[i]])
            {
                P[i]++;
            }
            if(i+P[i] > mx)
            {
                id = i;
                mx = i+P[i];
            }
        }
        mx = 0;
        for(int i = 0; i < s.length(); ++i)
        {
            if(P[i] > mx)
            {
                id = i;
                mx = P[i];
            }
        }
        for(int i = -P[id]+1; i < P[id]; ++i)
        {
            if(s[id+i] != '#')
            {
                re+=s[id+i];
            }
        }
        return re;
    }
};
```

#### Implement strStr() {#28}
经典的KMP，可以拿来练手，但是之前一直是`free(): invalid next size (fast):`,搞不懂，然后把next数组的长度加一就行了
``` cpp
class Solution {
public:
    void getNextArray(string &pattern, vector<int> &next)
    {
        int i = 0, j = -1;
        const int len = pattern.length();
        next[0] = -1;
        while(i < len)
        {
            if(j == -1 || pattern[i] == pattern[j])
            {
                i++;
                j++;
                next[i] = j;    
            }
            else
            {
                j = next[j];
            }
        }
    }
    int kmp(string &text, string &pattern,vector<int> &next)
    {
        const int m = text.length();
        const int n = pattern.length();
        int i = 0, j = 0;
        while(i < m && j < n)
        {
            if(j == -1 || text[i] == pattern[j])
            {
                i++;
                j++;
            }
            else
            {
                j = next[j];
            }
        }
        if(j == n)
            return i-j;
        else
            return -1;
    }
    int strStr(string haystack, string needle) {
        if(needle.length() == 0)
            return 0;
        vector<int> next(needle.length()+1,0);
        getNextArray(needle,next);
        return kmp(haystack,needle,next);
    }
};
```

#### Rotate Array {#189}
经典的字符串的三次翻转，但是要注意如果旋转的大小大于数组的大小的时候需要取余
``` cpp
class Solution {
public:
    void rotateRange(vector<int> &data, int start, int end)
    {
        while(start < end)
        {
            swap(data[start],data[end]);
            start++;
            end--;
        }
    }
    void rotate(vector<int>& nums, int k) {
        int len = nums.size();
        if(len <= k)
            k = k%len;
        rotateRange(nums,0,len-k-1);
        rotateRange(nums,len-k,len-1);
        rotateRange(nums,0,len-1);
    }
};
```
