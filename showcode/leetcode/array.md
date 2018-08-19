### 数组

#### Two Sum {#1}
使用pair 保存值和下标，然后按值进行排序（所以`make_pair`的时候值在前面），然后通过收尾指针进行夹逼目标值。
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

#### Median of Two Sorted Arrays {#4}
经典的在两个有序数组中查找第k个元素的题，按照二分的思路进行求解
1. A[k/2-1] < B[k/2-1] 则第k个元素一定不在 A[0:k/2-1] 故删除之，更新k
2. A[k/2-1] > B[k/2-1] 则第k个元素一定不在 B[0:k/2-1] 故删除之，更新k
3. A[k/2-1] == B[k/2-1] 则返回A[k/2-1] 
```
class Solution {
public:
    double findKth(vector<int>::const_iterator itea, vector<int>::const_iterator iteb, int m, int n, int k)
    {
        if(m > n)
            return findKth(iteb,itea,n,m,k);
        if(m == 0)
            return *(iteb+k-1);
        if(k == 1)
            return min(*itea, *iteb);

        int pa = min(k/2,m);
        int pb = k - pa;
        // 删除a数组的无效数据，并更新k
        if(*(itea+pa-1) < *(iteb+pb-1))
        {
            return findKth(itea+pa,iteb,m-pa,n,k-pa);
        }
        // 删除b数组的无效数据，并更新k
        else if(*(itea+pa-1) > *(iteb+pb-1))
        {
            return findKth(itea,iteb+pb,m,n-pb,k-pb);
        }
        else
            return *(itea+pa-1);
    }
    
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        const int m = nums1.size();
        const int n = nums2.size();
        int total = m+n;
        if(total & 0x1)
        {
            return findKth(nums1.begin(),nums2.begin(),m,n,total/2+1);
        }
        else
        {
            return (findKth(nums1.begin(),nums2.begin(),m,n,total/2)+findKth(nums1.begin(),nums2.begin(),m,n,total/2+1))/2.0;
        }
    }
};
```

#### Next Permutation {#31}
参考[Next Permutation 解题报告](http://fisherlei.blogspot.com/2012/12/leetcode-next-permutation.html) 主要分为下面四个步骤。

1. 找到反向的第一个递减下标作为分割点
2. 反向找到第一个比分割点大的交换点
3. 交换分割点与交换点的值
4. 从分割点到结尾整个区间的数翻转

需要注意的是当序列本来就是逆序时，直接翻转就好，例如 3,2,1 -> 1,2,3

``` cpp
void nextPermutation(vector<int>& nums) {
    if(nums.size() < 2)
    {
        return ;
    }
    vector<int>::iterator ite = nums.end()-2;
    for(;ite > nums.begin(); ite--)
    {
        if(*ite < *(ite+1))
            break;
    }
    if(ite == nums.begin() && *ite >= *(ite+1))
    {
        reverse(ite,nums.end());
        return;
    }
    vector<int>::iterator swapite;
    for(swapite = nums.end()-1; swapite > ite; swapite--)
    {
        if(*swapite > *ite)
            break;
    }
    swap(*ite,*swapite);
    reverse(ite+1,nums.end());
    return;
}
```

#### Longest Consecutive Sequence {#128}
由于时间复杂度的要求，所以使用hash表来存储一个段的最长的长度，通过循环剔除已经遍历过的数字

``` cpp
int longestConsecutive(vector<int>& nums) {
    int result = 0;
    unordered_map<int,bool> used;
    for(auto c:nums)
    {
        used[c] = false;
    }
    for(auto i:nums)
    {
        if(used[i])
            continue;
        int len = 1;
        used[i] = true;
        for(int j = i + 1; used.find(j) != used.end(); j++)
        {
            len++;
            used[j] = true;
        }
        for(int j = i - 1; used.find(j) != used.end(); j--)
        {
            len++;
            used[j] = true;
        }
        result = max(result,len);
    }
    return result;
}
```
