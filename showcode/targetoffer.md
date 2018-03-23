### 剑指Offer 刷题笔记
开始时间：2018.02.28

1. 二维数组中的查找 

  类似于二分的思路，先找到中间的数（广义中位数），然后每次缩小一大块空间
  ``` cpp
    bool Find(int target, vector<vector<int> > array) {
        const int line = array.size();
        if(line == 0)
            return false;
        const int row = array[0].size();
        int i = 0, j = line - 1;
        while(i < row && j >= 0)
        {
            if(target > array[i][j])
                i++;
            else if(target < array[i][j])
                j--;
            else
                return true;
        }
        return false;
    }
  ```

2. 替换空格

  从尾部进行处理以防止头部数据被覆盖 简单字符串处理
  ``` cpp
    void replaceSpace(char *str,int length)
    {
        if(str == nullptr)
            return ;
        int nonspace = 0;
        char *tmp = str;
        while(*tmp != '\0')
        {
            if(*tmp == ' ')
                nonspace++;
            tmp++;
        }
        for(int i = length-1; i >= 0; --i)
        {
            if(*(str+i) == ' ')
            {
                *(str+i+nonspace*2-2) = '%';
                *(str+i+nonspace*2-1) = '2';
                *(str+i+nonspace*2) = '0';
                nonspace--;
            }
            else
            {
                *(str+nonspace*2+i) = *(str+i);
            }
        }
        return ;
    }
  ```

3. 从尾到头打印链表

  简单递归处理链表
  
  ``` cpp
    vector<int> printListFromTailToHead(ListNode* head)
    {
        vector<int > re;
        printList(head,re);
        return re;
    
    }
    void printList(ListNode *node,vector<int > &re)
    {
        if(node == nullptr)
            return;
        printList(node->next,re);
        re.push_back(node->val);
    }
  ```

4. 重建二叉树

  递归问题，记得划分子问题的时候，子问题的解决方法应该与原问题一致
  ``` cpp
    #include <iostream>
    #include <vector>
    //// 记住 递归类的问题的子问题一定是与原问题给出的信息等价 也就是 子问题与原问题完全一致
    using namespace std;
    
    struct TreeNode {
        int val;
        TreeNode *left;
        TreeNode *right;
        TreeNode(int x) : val(x), left(NULL), right(NULL) {}
    };
    
    TreeNode* reCon(vector<int> pre,vector<int> vin, int pLeft, int pRight, int vStart, int vEnd)
    {
    
        if(pLeft > pRight || vStart > vEnd)
            return NULL;
        //cout<<pLeft<<" "<<pRight<<" "<<vStart<<" "<<vEnd<<endl;
        int count = 0;
        TreeNode* root = new TreeNode(pre[pLeft]);
        if(pLeft == pRight || vStart == vEnd)//到达叶子节点,不需要寻找左右节点
            return root;
    
        for(int i = vStart; i <= vEnd; ++i)
        {
            if(pre[pLeft] == vin[i])// 找到在中序遍历中的下标进行划分
            {
                break;
            }
            count++;
        }
        root->left = reCon(pre,vin,pLeft+1,pLeft+count,vStart,vStart+count-1);
        root->right = reCon(pre,vin,pLeft+count+1,pRight,vStart+count+1,vEnd);
        return root;
    
    }
    
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        TreeNode* root = reCon(pre,vin,0,pre.size()-1,0,vin.size()-1);
        return root;
    }
    int main(int argc, char *argv[])
    {
        vector<int > pre = {1,2,4,7,3,5,6,8};
        vector<int > vin = {4,7,2,1,5,3,8,6};
        reConstructBinaryTree(pre,vin);
    
        return 0;
    }

  ```

5. 用两个栈实现队列

  简单的栈处理 一个用于push 一个用于pop
  ``` cpp
    class Solution
    {
    public:
        void push(int node) {
            stack1.push(node);
        }
    
        int pop() {
            if(stack2.size() < 1)
            {
                while(stack1.size() > 0)
                {
                    stack2.push(stack1.top());
                    stack1.pop();                
                }
    
            }
            int tmp = stack2.top();
            stack2.pop();
            return tmp;
    
        }
    
    private:
        stack<int> stack1;
        stack<int> stack2;
    };
  ```

6. 旋转数组的最小数字

  简单的二分思想的运用，当一个序列可以用函数的单调性来描述的时候就会使用二分
  但是要注意单调不减 所以注意等号的处理
  ``` cpp
    int findMinNum(vector<int> &rArr, int start, int end)
    {
        if(end - start < 2)
        {
            return min(rArr[start],rArr[end]);
        }
        int middle = (start+end)/2;
        if(rArr[middle] >= rArr[start] && rArr[middle] >= rArr[end])
        {
            return findMinNum(rArr,middle,end);
        }
        else if(rArr[middle] <= rArr[start] && rArr[middle] <= rArr[end])
        {
            return findMinNum(rArr,start,middle);
        }
        else if(rArr[middle] <= rArr[start] && rArr[middle] >= rArr[end])
        {
            return rArr[middle];
        }
    
        return 0;
    }
    int minNumberInRotateArray(vector<int> rotateArray)
    {
        if(rotateArray.size() < 1)
            return 0;
        return findMinNum(rotateArray,0,rotateArray.size()-1);
    }
  ```

7. 斐波那契数列

  非常简单的递归转动态规划(或迭代)
  ``` cpp
    int Fibonacci(int n) {
       int i = 3;
       int f1 = 1, f2 = 1;
       if(n == 0)
           return 0;
       else if(n < 3)
           return 1;
       int tmp;
       while(i++ <= n)
       {
           tmp = f2;
           f2 = f1 + f2;
           f1 = tmp;
       }
       return f2;
    
    }
  ```

8. 跳台阶

  上题的变种
  ``` cpp
    int jumpFloor(int number) {
        if(number <= 0)
            return 0;
        else if(number == 1)
            return 1;
        else if(number == 2)
            return 2;
        int f1 = 1, f2 = 2;
        int tmp, index = 3;
        while(index++ <= number)
        {
            tmp = f2;
            f2 = f1+f2;
            f1 = tmp;
        }
        return f2;
    
    }
  ```

9. 变态跳台阶

  数学归纳法找规律的题
  ``` cpp
    int jumpFloorII(int number) {
        if(number <= 0)
            return 0;
        else
            return pow(2,number-1);
    }
  ```

10. 矩形覆盖

  斐波那契数列变种
  ``` cpp
    int rectCover(int number) {
        if(number <= 0)
            return 0;
        else if(number == 1)
            return 1;
        else if(number == 2)
            return 2;
        else
        {
            int index = 3;
            int tmp,f1 = 1, f2 = 2;
            while(index++ <= number)
            {
                tmp = f2;
                f2 = f1+f2;
                f1 = tmp;
            }
            return f2;
        }
    }
  ```

11. 二进制中1的个数

  简单的数值处理 记得有负数不能用移位然后与1与的方法
  ``` cpp
    int  NumberOf1(int n) {
         int count = 0;
         while(n)
         {
             count++;
             n&=(n-1);
         }
         return count;
     }
  ```

12. 数值的整数次方

  简单数值处理（也是二分） 注意细节 指数为0 指数为负 指数为奇数
  ``` cpp
    double Power(double base, int exponent) {
        bool neg = exponent > 0 ? 0 : 1;
        bool ood = exponent % 2;
        exponent = abs(exponent);
        double re = base;
        if(exponent == 0)
            re = 1;
        else if(exponent != 1)
        {
            while(exponent>1)
            {
                re = re*re;
                exponent/=2;
            }
            if(ood)
                re = re*base;
        }
        return neg ? 1/re : re;
    }
  ```

13. 调整数组顺序使奇数位于偶数前面

  数组 处理 在空间不允许的情况下（不要求位置不变） 可以 用两个指针 指向头和尾，然后交换

  ``` cpp
    void reOrderArray(vector<int> &array) {
        vector<int> re;
        for(int i = 0; i < array.size();++i)
        {
            if(array[i] % 2)
                re.push_back(array[i]);
        }
        for(int i = 0; i < array.size();++i)
        {
            if(array[i] % 2 == 0)
                re.push_back(array[i]);
        }
        array = re;
    }
  ```

14. 链表中倒数第k个结点

  简单的链表处理，头尾两个指针进行处理
  ``` cpp
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode* tmp = pListHead;
        for(int i = 0; i < k; ++i)
        {
            if(tmp == NULL)
                return NULL;
            tmp = tmp->next;
        }
        while(tmp!= NULL)
        {
            tmp = tmp->next;
            pListHead = pListHead->next;
        }
        return pListHead;
    }
  ```

15. 反转链表

  中等难度的链表处理，千万要注意细节，就是指针为空的时候
然后有递归（难理解 简洁）和迭代两种方式来实现
  ``` cpp
    #include <iostream>
    
    using namespace std;
    
    // 反转链表
    struct ListNode {
        int val;
        struct ListNode *next;
        ListNode(int x) :
            val(x), next(NULL) {
        }
    };
    
    // 非递归实现，注意空指针的判断，否则直接段错误
    ListNode* ReverseList(ListNode* pHead) {
        if(pHead == nullptr)
            return nullptr;
        ListNode *pre = NULL,*tmp2;
        while(pHead->next != NULL)
        {
            tmp2 = pHead->next;
            pHead->next = pre;
            pre = pHead;
            pHead = tmp2;
        }
        pHead->next = pre;
        return pHead;
    }
    // 递归版
    ListNode* RecReverseList(ListNode* pHead) {
        if(pHead->next == nullptr || pHead == nullptr)
        {
            return pHead;
        }
        ListNode *pre = RecReverseList(pHead->next);
        // 注意每次 pHead 这个局部变量的存在，都是指向之前的指针，相当于每次pHead 的值是栈顶的值
        pHead->next->next = pHead;
        pHead->next = nullptr;
        return pre;
    }
    
    int main(int argc, char *argv[])
    {
        ListNode *pHead = new ListNode(0);
        ListNode *ptmp = pHead;
        for(int i = 1; i < 6; ++i)
        {
            ptmp->next = new ListNode(i);
            ptmp = ptmp->next;
        }
        RecReverseList(pHead);
    
        return 0;
    }

  ```

16. 合并两个排序的链表

  简单的链表处理
  ``` cpp

    ListNode* Merge(ListNode* pHead1, ListNode* pHead2)
    {
        if(pHead1 == nullptr && pHead2 == nullptr)
            return nullptr;
        else if(pHead1 != nullptr && pHead2 == nullptr)
            return pHead1;
        else if(pHead1 == nullptr && pHead2 != nullptr)
            return pHead2;
    
        ListNode *head, *tmp;
        if(pHead1->val < pHead2->val)
        {
            head = pHead1;
            pHead1 = pHead1->next;
        }
        else
        {
            head = pHead2;
            pHead2 = pHead2->next;
        }
    
        tmp = head;
        while(pHead1 != nullptr && pHead2 != nullptr)
        {
            if(pHead1->val < pHead2->val)
            {
                tmp->next = pHead1;
                tmp = pHead1;
                pHead1 = pHead1->next;
            }
            else
            {
                tmp->next = pHead2;
                tmp = pHead2;
                pHead2 = pHead2->next;
            }           
        }
        if(pHead1 != nullptr)
        {
            tmp->next = pHead1;
        }
        else
        {
            tmp->next = pHead2;
        }
        return head;

  ```

17. 树的子结构
  
  dssdsd
  ``` cpp
    add
  ```

18. 二叉树的镜像
  
  aaa
  ``` cpp
    sdafas
  ```

结束时间：2018.03.xx