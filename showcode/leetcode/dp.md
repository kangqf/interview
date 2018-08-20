#### Maximum Subarray {#53}
简单的动态规划 MCSS 问题
``` cpp
int maxSubArray(vector<int>& nums) {
    vector<int> dp(nums.size(),0);
    dp[0] = nums[0];
    for(int i = 1; i < nums.size(); ++i)
    {
        if(dp[i-1] > 0)
            dp[i] = dp[i-1] + nums[i];
        else
            dp[i] = nums[i];
    }
    int maxRe = INT_MIN;
    for(auto c:dp)
    {
        if(c > maxRe)
            maxRe = c;
    }
    return maxRe;
}
```

#### Edit Distance {#72}
经典的字符串的编辑距离的问题
``` cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        const int m = word1.length();
        const int n = word2.length();
        vector<vector<int>> dp(m+1,vector<int>(n+1,0));
        for(int i = 0; i <= m; ++i)
        {
            dp[i][0] = i;
        }
        for(int i = 0; i <= n; ++i)
        {
            dp[0][i] = i;
        }
        for(int i = 1; i <= m; ++i)
        {
            for(int j = 1; j <= n; ++j)
            {
                if(word1[i-1] == word2[j-1])
                {
                    dp[i][j] = dp[i-1][j-1];
                }
                else
                {
                    dp[i][j] = min(min(dp[i-1][j-1],dp[i-1][j]),dp[i][j-1])+1;
                }
            }
        }
        return dp[m][n];
    }
};
```

#### Edit Distance {#72}
经典的字符串的编辑距离的问题
``` cpp

```
