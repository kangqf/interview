## STL 中一些函数的使用技巧

### 从vector 到 set
C++ STL中标准关联容器set, multiset, map, multimap内部采用的就是一种非常高效的平衡二叉查找树：红黑树
1. 通过初始化的方式
    ``` cpp
        std::vector<int> nums1 {3, 1, 4, 6, 5, 9};
        //使用初始化的方式从vector 到 set
        std::set<int> numSet(nums1.begin(),nums1.end());
    ```
2. 通过插入迭代器算法拷贝的方式
    ``` cpp 
        std::vector<int> nums1 {3, 1, 4, 6, 5, 9};
         // 使用迭代器的方式从 vector 到 set
        set<int> numSet;
        copy(nums1.begin(),nums1.end(),std::inserter(numSet,numSet.begin()));
    ```