#### Implement strStr(){#28}
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

#### Rotate Array{#189}
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
