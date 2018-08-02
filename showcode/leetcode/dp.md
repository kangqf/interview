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
