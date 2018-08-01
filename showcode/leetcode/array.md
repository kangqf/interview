### 数组

#### Two Sum {#1}
使用pair 保存值和下标，然后按值进行排序（所以make_pair的时候值在前面），然后通过收尾指针进行夹逼目标值。
``` cpp
vector<int> twoSum(vector<int>& nums, int target) 
{
        int beg = 0;
        int end = nums.size()-1;
        vector<int> re;
        vector<pair<int,int>> data;
        for(int i = 0; i < nums.size(); ++i)
        {
            data.push_back(make_pair(nums[i],i));
        }
        sort(data.begin(),data.end());
        while(beg < end)
        {
            if(data[beg].first + data[end].first == target)
            {
                re.push_back(data[beg].second);
                re.push_back(data[end].second);
                break;
            }
            else if(data[beg].first + data[end].first < target)
            {
                beg++;
            }
            else
            {
                end--;
            }
        }
        return re;
    }
```