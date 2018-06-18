### 剑指Offer
1. 二维数组中的查找

    在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

    ``` cpp
    class Solution {
    public:
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
    };
    ```

2. 替换空格

    请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

    ``` cpp
    class Solution {
    public:
    	void replaceSpace(char *str,int length) {
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
    };
    ```

3. 从尾到头打印链表

    输入一个链表，从尾到头打印链表每个节点的值。

    ``` cpp
    
    /**
    *  struct ListNode {
    *        int val;
    *        struct ListNode *next;
    *        ListNode(int x) :
    *              val(x), next(NULL) {
    *        }
    *  };
    */
    class Solution {
    public:
        vector<int> printListFromTailToHead(ListNode* head) {
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
    };
    ```

4. 重建二叉树

    输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

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

    用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

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

    把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

    ``` cpp
    class Solution {
    public:
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
        
    };
    ```

7. 斐波那契数列

    大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。n<=39

    ``` cpp
    class Solution {
    public:
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
    };
    ```

8. 跳台阶

    一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

    ``` cpp
    class Solution {
    public:
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
    };
    ```

9. 变态跳台阶

    一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

    ``` cpp
    class Solution {
    public:
        int jumpFloorII(int number) {
            if(number <= 0)
                return 0;
            else 
                return pow(2,number-1);
        }
    };
    ```

10. 矩形覆盖

    我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

    ``` cpp
    class Solution {
    public:
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
    };
    ```

11. 二进制中1的个数

    输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

    ``` cpp
    class Solution {
    public:
         int  NumberOf1(int n) {
             int count = 0;
             while(n)
             {
                 count++;
                 n&=(n-1);
             }
             return count;
         }
    };
    ```

12. 数值的整数次方 

    给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

    ``` cpp
    class Solution {
    public:
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
    };
    ```

13. 调整数组顺序使奇数位于偶数前面

    输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

    ``` cpp
    class Solution {
    public:
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
    };
    ```

14. 链表中倒数第k个结点

    输入一个链表，输出该链表中倒数第k个结点。

    ``` cpp
    /*
    struct ListNode {
    	int val;
    	struct ListNode *next;
    	ListNode(int x) :
    			val(x), next(NULL) {
    	}
    };*/
    class Solution {
    public:
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
    };
    ```

15. 反转链表

    输入一个链表，反转链表后，输出链表的所有元素。

    ``` cpp
    #include <iostream>
    
    using namespace std;
    
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

    输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

    ``` cpp
    /*
    struct ListNode {
    	int val;
    	struct ListNode *next;
    	ListNode(int x) :
    			val(x), next(NULL) {
    	}
    };*/
    class Solution {
    public:
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
        }
    };
    ```

17. 树的子结构

    输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

    ``` cpp
    /*
    struct TreeNode {
    	int val;
    	struct TreeNode *left;
    	struct TreeNode *right;
    	TreeNode(int x) :
    			val(x), left(NULL), right(NULL) {
    	}
    };*/
    class Solution {
    public:
        bool isSameTree(TreeNode* pRoot1, TreeNode* pRoot2)
        {
            if(pRoot1 == nullptr || pRoot2 == nullptr)
                return false;
            while(pRoot1->)
        }
        bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
        {
            if(pRoot2 == nullptr)
                return false;
        }
    };
    ```

18. 二叉树的镜像

    操作给定的二叉树，将其变换为源二叉树的镜像。

    ``` cpp
    /*
    struct TreeNode {
    	int val;
    	struct TreeNode *left;
    	struct TreeNode *right;
    	TreeNode(int x) :
    			val(x), left(NULL), right(NULL) {
    	}
    };*/
    class Solution {
    public:
        void Mirror(TreeNode *pRoot) {
            if(pRoot == nullptr)
                return ;
            else
            {
                Mirror(pRoot->left);
                Mirror(pRoot->right);
            }
            TreeNode *tmp = pRoot->left;
            pRoot->left = pRoot->right;
            pRoot->right = tmp;
        }
    };
    ```

19. 顺时针打印矩阵

    输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字

    ``` cpp
    class Solution {
    public:
    
        vector<int> printMatrix(vector<vector<int> > matrix) {
            vector<int> re;
            if(matrix.size() == 0)
                return re;
            
            int xStart = 0, xEnd = matrix[0].size()-1;
            int yStart = 0, yEnd = matrix.size()-1;
            while(xStart <= xEnd && yStart <= yEnd)
            {
                int i = xStart,j = yStart;
                for(i = xStart; i <= xEnd; ++i)
                {
                    re.push_back(matrix[yStart][i]);
                }
                
                for(j = yStart+1; j <= yEnd; j++)
                {
                    re.push_back(matrix[j][xEnd]);
                }
                if(yStart < yEnd)
                for(i = xEnd-1; i >= xStart; --i)
                {
                    re.push_back(matrix[yEnd][i]);
                }
                if(xStart < xEnd)
                for(j = yEnd-1; j >= yStart+1; --j)
                {
                    re.push_back(matrix[j][xStart]);
                }
                yEnd--;
                xEnd--;
                yStart++;
                xStart++;
            }
    
            return re;
        }
    };
    ```

20. 包含min函数的栈

    定义栈的数据结构，请在该类型中实现一个能够得到栈最小元素的min函数。

    ``` cpp
    class Solution {
    public:
        
        stack<int> normal;
        stack<int> mins;
        
        void push(int value) {
            normal.push(value);
            if(mins.size() == 0 || value < mins.top())
            {
                mins.push(value);
            }
        }
        void pop() {
            if(normal.top() == mins.top())
            {
                mins.pop();
            }
            normal.pop();
        }
        int top() {
            return normal.top();
        }
        int min() {
            return mins.top();
        }
    };
    ```

21. 栈的压入、弹出序列

    输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4，5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

    ``` cpp
    class Solution {
    public:
        bool IsPopOrder(vector<int> pushV,vector<int> popV) {
            if(pushV.size() == 0)
            return false;
    
            const int pushSize = pushV.size();
            int indexPush = 0;
            int indexPop = 0;
            stack<int> pushS;
    
            while(indexPop < pushSize)
            {
                if(pushS.size() == 0 || pushS.top() != popV[indexPop])
                {
                    if(indexPush == pushSize)
                        return false;
                    else
                        pushS.push(pushV[indexPush++]);
                }
    
                if(pushS.top() == popV[indexPop])
                {
                    pushS.pop();
                    indexPop++;
                }
            }
            return true;
        }
    };
    ```

22. 从上往下打印二叉树

    从上往下打印出二叉树的每个节点，同层节点从左至右打印。

    ``` cpp
    /*
    struct TreeNode {
    	int val;
    	struct TreeNode *left;
    	struct TreeNode *right;
    	TreeNode(int x) :
    			val(x), left(NULL), right(NULL) {
    	}
    };*/
    class Solution {
    public:
        vector<int> PrintFromTopToBottom(TreeNode* root) {
            vector<int >re;
            if(root == nullptr)
                return re;
           
            queue<TreeNode* > prt;
            prt.push(root);
            while(prt.size() != 0)
            {
                TreeNode* node = prt.front();
                prt.pop();
                re.push_back(node->val);
                if(node->left != nullptr)
                {
                    prt.push(node->left);
                }
                if(node->right != nullptr)
                {
                    prt.push(node->right);
                }
            }
            
            return re;
    
        }
    };
    ```

23. 二叉搜索树的后序遍历序列

    输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

    ``` cpp
    class Solution {
    public:
        bool verifySqu(int start, int end, vector<int> &seq)
        {
            if(start >= end)//注意start可能会产生大于 end 的情况 比如 start = 1 end = 2 i = 1 调用 （i+1，end-1） （2，1）
                return true;
            int i = end - 1;
            for( ; seq[i] > seq[end] && i >= start; --i)
                ;
            for(int j = i; j >= start; --j)
            {
                if(seq[j] > seq[end])
                    return false;
            }
            return (verifySqu(start,i,seq) && verifySqu(i+1,end-1,seq));
        }
        
        bool VerifySquenceOfBST(vector<int> sequence) {
            const int seqSize = sequence.size();
            if(seqSize == 0)
            {
                return false;
            }
            
            return verifySqu(0, seqSize-1, sequence);
        }
    };
    ```

24. 二叉树中和为某一值的路径

    输入一颗二叉树和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。

    ``` cpp
    /*
    struct TreeNode {
    	int val;
    	struct TreeNode *left;
    	struct TreeNode *right;
    	TreeNode(int x) :
    			val(x), left(NULL), right(NULL) {
    	}
    };*/
    class Solution {
    public:
        void getPath(TreeNode* root,int expectNumber, int  currentSum, vector<vector<int> > &re, vector<int> &path)
        {
            path.push_back(root->val);
            currentSum += root->val;
            if(currentSum == expectNumber && root->left == nullptr && root->right == nullptr)
            {
                re.push_back(path);
            }
            else
            {
                if(root->left != nullptr)
                {
                    getPath(root->left,expectNumber,currentSum,re,path);
                }
                
                if(root->right != nullptr)
                {
                    getPath(root->right,expectNumber,currentSum,re,path);
                }
            }
            path.pop_back();
            currentSum -= root->val;
        }
        vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
            vector<vector<int> > re;
            vector<int> path;
            int currentSum = 0;
            if(root == nullptr)
                return re;
            getPath(root,expectNumber,currentSum,re,path);
            return re;
        }
    };
    ```

25. 复杂链表的复制

    输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

    ``` cpp
    /*
    struct RandomListNode {
        int label;
        struct RandomListNode *next, *random;
        RandomListNode(int x) :
                label(x), next(NULL), random(NULL) {
        }
    };
    */
    class Solution {
    public:
        RandomListNode* Clone(RandomListNode* pHead)
        {
            if(pHead == nullptr)
                return nullptr;
            
            RandomListNode* node = pHead;
            while(node != nullptr)
            {
                RandomListNode* tmp = new RandomListNode(node->label);
                tmp->next = node->next;
                node->next = tmp;
                node = tmp->next;
            }

            node = pHead;
            
            while(node != nullptr)
            {
                if(node->random != nullptr)//注意这个点
                    node->next->random = node->random->next;
                node = node->next->next;
            }
            
            node = pHead;
            RandomListNode* pHeadClo = pHead->next;
            RandomListNode* nodeClo;
            while(node->next != nullptr)
            {
                nodeClo = node->next;
                node->next = nodeClo->next;
                node = nodeClo;
            }
            return pHeadClo;
        }
    };
    ```

26. 二叉搜索树与双向链表

    输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向

    ``` cpp
    /*
    struct TreeNode {
    	int val;
    	struct TreeNode *left;
    	struct TreeNode *right;
    	TreeNode(int x) :
    			val(x), left(NULL), right(NULL) {
    	}
    };*/
    class Solution {
    public:
        void ConverTree(TreeNode *node, TreeNode *&lastNode)
        {
            if(node == nullptr)
            {
                return;
            }
            
            ConverTree(node->left, lastNode);
            
            if(lastNode != nullptr)
            {
                lastNode->right = node;
            }
            node->left = lastNode;
            lastNode = node;
            
            ConverTree(node->right, lastNode);
        }
        
        TreeNode* Convert(TreeNode* pRootOfTree)
        {
            if(pRootOfTree == nullptr)
            {
                return nullptr;
            }
            TreeNode *lastNode = nullptr;
            
            ConverTree(pRootOfTree,lastNode);
            
            TreeNode *head = pRootOfTree;
            while(head->left != nullptr)
                head = head->left;
            return head;
        }
    };
    ```

27. 字符串的排列

    输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

    ``` cpp
    class Solution {
    public:
        void perm(vector<string> &re, string &str, int index)
        {
            for(int i = index; i < str.length(); ++i)
            {
                if(index == str.length()-1)
                {
                    re.push_back(str);
                    return;
                }
    
                if( i != index && str[i] == str[index])
                    continue;
                swap(str[index],str[i]);
                if(index+1 < str.length())
                    perm(re,str,index+1);
                swap(str[index],str[i]);
            }
        }
        vector<string> Permutation(string str) {
            vector<string> re;
            if(str.length() == 0)
                return re;
            
            //re.push_back(str);
            perm(re,str,0);
            sort(re.begin(),re.end());
            return re;
        }
    };
    ```

28. 数组中出现次数超过一半的数字

    数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0

    ``` cpp
    class Solution {
    public:
        int MoreThanHalfNum_Solution(vector<int> numbers) {
            if(numbers.size() == 0)
                return 0;
            int num = numbers[0];
            int count = 0;
            for(int i = 0; i < numbers.size(); ++i)
            {
                if(count == 0)
                {
                    num = numbers[i];
                    count++;
                }
                else
                {
                    if(numbers[i] == num)
                        count++;
                    else 
                        count--;
                }
            }
            count = 0;
            for(int i = 0; i < numbers.size(); ++i)
            {
                if(numbers[i] == num)
                    count++;
            }
            if(count > (numbers.size())/2)
                return num;
            else
                return 0;
        }
    };
    ```

29. 最小的K个数

    输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

    ``` cpp
    class Solution {
    public:
        vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
            vector<int> re;
            if(input.size() < k)
                return re;
            set<int> inSet(input.begin(),input.end());
            int i = 0;
            for(auto ite = inSet.begin(); ite != inSet.end(); ite++)
            {
                if(i++ == k)
                    break;
                re.push_back(*ite);
            }
            return re;
        }
    };
    ```

30. 连续子数组的最大和

    HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。你会不会被他忽悠住？(子向量的长度至少是1)

    ``` cpp
    class Solution {
    public:
        int FindGreatestSumOfSubArray(vector<int> array) {
            int maxSum = array[0], curr = 0;
            for(int i = 0; i < array.size(); ++i)
            {
                if(curr < 0)
                    curr = array[i];
                else
                    curr += array[i];
                maxSum = max(maxSum,curr);
            }
            return maxSum;
        }
    };
    ```

31. 整数中1出现的次数

    求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数。

    ``` cpp
    class Solution {
    public:
        int NumberOf1Between1AndN_Solution(int n)
        {
            if(n < 1)
                return 0;
            int i = 1, low = 0, curr = 0, high = 0;
            int value = n;
            int count = 0;
            int pow[] = {1,10,100,1000,10000,100000,1000000};
            while(value)
            {
                curr = value % 10;
                high = value / 10;
                value = high;
                
                if(curr > 1)
                    count += (high + 1) * pow[i-1];
                else if(curr == 1)
                    count += high * pow[i-1] + low +1;
                else
                    count += high * pow[i-1];
    
                low += curr*pow[i-1];
                i++;
            }
            return count;
        }
    };
    ```

32. 把数组排成最小的数

    输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

    ``` cpp
    class Solution {
    public:
        string PrintMinNumber(vector<int> numbers) {
            if(numbers.size() == 0)
                return "";
            
            vector<string> strVec;
            for(auto num:numbers)
                strVec.push_back(to_string(num));
            sort(strVec.begin(),strVec.end(),[](string s1, string s2) -> bool {
                return s1+s2 < s2+s1;
            });
            string re;
            for(auto str:strVec)
                re += str;
            return re;
        }
    };
    ```

33. 丑数

    把只包含因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

    ``` cpp
    class Solution {
    public:
        int GetUglyNumber_Solution(int index) {
            if(index == 1)
                return 1;
            vector<int> ugly;
            ugly.push_back(1);
            int m2 = 0,m3 = 0,m5 = 0;
            int u2 = 1,u3 = 1,u5 =1;
            int minval = 1;
            int i;
            for(i = 0; i < index; i++)
            {
                u2 = ugly[m2]*2;
                u3 = ugly[m3]*3;
                u5 = ugly[m5]*5;
                minval = min(u2,min(u3,u5));
                ugly.push_back(minval); 
                if(minval == u2)
                {
                    m2++;
                }
                if(minval == u3)//注意是if而不是else if 因为两个数可能会通过先后顺序不一样乘到一样的 2*3 3*2
                {
                    m3++;
                }
                if(minval == u5)
                {
                    m5++;
                }
            }
            return ugly[i-1];
        }
    };
    ```

34. 第一个只出现一次的字符位置

    在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置

    ``` cpp
    class Solution {
    public:
        int FirstNotRepeatingChar(string str) {
            map<char,int> dic;
            char lastc;
            int index = 0;
            for(auto c:str)
            {
                dic[c]++;
            }
            for(auto c:str)
            {
                index++;
                if(dic[c] == 1)
                    return index-1;
            }
            return -1;
        }
    };
    ```

35. 数组中的逆序对

    在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

    ``` cpp
    class Solution {
    public:
        long long InverseCount(vector<int> &arr, vector<int> &cpy, int start, int end)
        {
            if(start == end)
            {
                cpy[start] = arr[start];
                // 只有一个元素，逆序数为 0
                return 0;
            }
    
            int len = (end - start)/2;
            // 统计左半边数组的逆序数
            // 这里较难理解的是 cpy 与 arr 的顺序的问题 也就是 arr与cpy交替使用的问题
            // 我们可以这样理解： 传给下面的递归的 第二个参数（作为下层递归的cpy） 返回后会使有序的，会作为本轮的arr，因此 arr就是有序的
            long leftCount = InverseCount(cpy,arr,start,start+len);
            // 统计右半边数组的逆序数
            long rightCount = InverseCount(cpy,arr,start+len+1,end);
    
            // 前半段最后数字下标
            int i = start+len;
            // 后半段最后数字下标
            int j = end;
            // 开始拷贝位置
            int indexCpy = end;
            // 逆序数
            long long count = 0;
            while(i >= start && j >= start+len+1)
            {
                if(arr[i] > arr[j])
                {
                    // 前面数组最大值 比 后面数组最大值大 所以后面最大值前面的都构成逆序数对
                    // 也就是说连最大的比过了，那么前面的小的必然可以比的过
                    count += j-start-len;
                    // 保存合并数组后的最大值
                    cpy[indexCpy] = arr[i];
                    // 寻找前半部分数组中较小的
                    i--;
                    // 存放最大数的下标往前移动
                    indexCpy--;
                }
                else
                {
                    // 不构成逆序数，然后复制到辅助数组就好
                    cpy[indexCpy--] = arr[j--];
                }
            }
            // 有可能某部分数组没有遍历完，则完成该部分数组数据的拷贝
            while(i >= start)
            {
                cpy[indexCpy--] = arr[i--];
            }
            while(j >= start+len+1)
            {
                cpy[indexCpy--] = arr[j--];
            }
    
            return (count+leftCount+rightCount)%1000000007;
        }
        int InversePairs(vector<int> data) {
            vector<int> cpy(data.size(),0);
            for(int i = 0; i < data.size(); ++i)
            {
                cpy[i] = data[i];
            }
            return InverseCount(data,cpy,0,data.size()-1) % 1000000007;
        }
    };
    ```

36. 两个链表的第一个公共结点

    输入两个链表，找出它们的第一个公共结点

    ``` cpp
    /*
    struct ListNode {
    	int val;
    	struct ListNode *next;
    	ListNode(int x) :
    			val(x), next(NULL) {
    	}
    };*/
    class Solution {
    public:
        ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
            auto it1 = pHead1, it2 = pHead2;
            while(it1 != it2)
            {
                it1 = (it1 == nullptr ? pHead1 : it1->next);
                it2 = (it2 == nullptr ? pHead2 : it2->next);
            }
            return it1;
            
        }
    };
    ```

37. 数字在排序数组中出现的次数

    统计一个数字在排序数组中出现的次数。

    ``` cpp
    class Solution {
    public:
        int findFirstK(vector<int> &data, int &k, int start, int end)
        {
            if(start > end)
                return -1;
            int middle = start + (end - start)/2;
            if(data[middle] == k)
            {
                if(middle == 0 || data[middle-1] != k)
                {
                    return middle;
                }
                else
                {
                    return findFirstK(data,k,start,middle-1);
                }
            }
            else if(data[middle] > k)
            {
                return findFirstK(data,k,start,middle-1);
            }
            else
            {
                return findFirstK(data,k,middle+1,end);
            }
        }
        int findLastK(vector<int> &data, int &k, int start, int end)
        {
            if(start > end)
                return -1;
            int middle = start + (end - start)/2;
            if(data[middle] == k)
            {
                if(middle == data.size()-1 || data[middle+1] != k)
                {
                    return middle;
                }
                else
                {
                    return findLastK(data,k,middle+1,end);
                }
            }
            else if(data[middle] > k)
            {
                return findLastK(data,k,start,middle-1);
            }
            else
            {
                return findLastK(data,k,middle+1,end);
            }
        }
        int GetNumberOfK(vector<int> data ,int k) {
            // 注意这里要处理 没有的情况 以及只有一个的情况
            int last = findLastK(data,k,0,data.size()-1);
            int first = findFirstK(data,k,0,data.size()-1);
            return first == -1 ? 0 : last-first+1;
        }
    };
    ```

38. 二叉树的深度

    输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

    ``` cpp
    /*
    struct TreeNode {
    	int val;
    	struct TreeNode *left;
    	struct TreeNode *right;
    	TreeNode(int x) :
    			val(x), left(NULL), right(NULL) {
    	}
    };*/
    class Solution {
    public:
        int TreeDepth(TreeNode* pRoot)
        {
            if(pRoot == nullptr)
                return 0;
            if(pRoot->left == nullptr && pRoot -> right == nullptr)
                return 1;
            int left = TreeDepth(pRoot->left);
            int right = TreeDepth(pRoot->right);
            return left > right ? left + 1 : right + 1;
        }
    };
    ```

39. 平衡二叉树

    输入一棵二叉树，判断该二叉树是否是平衡二叉树。

    ``` cpp
    class Solution {
    public:
        bool IsBalanced(TreeNode* pRoot, int & height)
    	{
    		if(pRoot == nullptr)
    		{
    			height = 0;
    			return true;
    		}
    		
    		int left = 0;
    		if(!IsBalanced(pRoot->left,left))
    			return false;
    		int right = 0;
    		if(!IsBalanced(pRoot->right,right))
    			return false;
    		
    		if(abs(left - right) > 1)
    			return false;
    		
    		height = max(left,right) + 1;
    		
    		return true;
    		
    	}
        bool IsBalanced_Solution(TreeNode* pRoot) {
            if(pRoot == nullptr)
    		{
    			return true;
    		}
    		else
    		{
    			int height = 0;
    			return IsBalanced(pRoot,height);
    		}
        }
    };
    ```

40. 数组中只出现一次的数字

    一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

    ``` cpp
    class Solution {
    public:
        void FindNumsAppearOnce(vector<int> data,int* num1,int *num2) {
            int xorVal = 0;
    		for(auto it : data)
    		{
    			xorVal ^= it;
    		}
    		int i = 0;
    		for(i = 0; i < 32; ++i)
    		{
    			if(xorVal & 0x01)//寻找第一个为1 的位
    			{
    				break;
    			}
    			else
    			{
    				xorVal >>= 1;
    			}
    		}
    		int j = 0;
    		vector<int> data1,data2;
    		for(auto it:data)
    		{
    			if((it >> i) & 0x01)
    			{
    				data1.push_back(it);
    			}
    			else
    			{
    				data2.push_back(it);
    			}
    		}
    		
    		for(auto it:data1)
    		{
    			*num1 ^= it;
    		}
    		
    		for(auto it:data2)
    		{
    			*num2 ^= it;
    		}
    		
    		return;
        }
    };
    ```

41. 和为S的连续正数序列

    小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

    ``` cpp
    class Solution {
    public:
        vector<vector<int> > FindContinuousSequence(int sum) {
            vector<vector<int> > re;
    		for(int i = 1,j = 1; j <= sum/2+1; i <= sum /2)
    		{
    			if((i+j)*(j-i+1)/2 < sum)
    			{
    				j++;
    			}
    			else if((i+j)*(j-i+1)/2 == sum)
    			{
    				vector<int> tmp;
    				for(int k = i; k <= j; ++k)
    				{
    					tmp.push_back(k);
    				}
                    if(j-i > 0)
    				re.push_back(tmp);
                    j++;
    			}
    			else if((i+j)*(j-i+1)/2 > sum)
    			{
    				i++;
    			}
    		}
            return re;
        }
    };
    ```

42. 和为S的两个数字

    输入一个递增排序的数组和一个数字S，在数组中查找两个数，是的他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

    ``` cpp
    class Solution {
    public:
        vector<int> FindNumbersWithSum(vector<int> array,int sum) {
             int num1 = 0, num2 = 0;
    		 for(int i = 0, j = array.size()-1; i < j;)
    		 {
    			if(array[i] + array[j] == sum)
    			{
    				if(array[i]*array[j] < num1*num2 || (num1 == 0 && num2 ==0))
    				{
    					num1 = array[i];
    					num2 = array[j];
    				}
    				i++;
    			}
    			else if(array[i] + array[j] < sum)
    			{
    				i++;
    			}
    			else if(array[i] + array[j] > sum)
    			{
    				j--;
    			}
    		 }
    		 
    		 vector<int> re;
    		 if(num1 != 0 && num2 != 0)
    		 {
    			re.push_back(num1);
    			re.push_back(num2);
    		 }
    		 return re;
        }
    };
    ```

43. 左旋转字符串

    汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

    ``` cpp
    class Solution {
    public:
        void reverse(string &str,int start,int end)
    	{
    		for(int i = start, j = end; i < j; ++i,--j)
    		{
    			swap(str[i],str[j]);
    		}
    	}
    	
    	string LeftRotateString(string str, int n) {
            if(str.length() < 2)
                return str;
            reverse(str,0,n-1);
    		reverse(str,n,str.length()-1);
    		reverse(str,0,str.length()-1);
    		return str;
        }
    };
    ```

44. 翻转单词顺序列

    牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

    ``` cpp
    class Solution {
    public:
        string ReverseSentence(string str) {
            int pos = str.find(" ");
    		return pos == string::npos ? str : ReverseSentence(str.substr(pos+1)) + " " + str.substr(0,pos);
        }
    };
    ```

45. 扑克牌顺子

    LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何。为了方便起见,你可以认为大小王是0

    ``` cpp
    class Solution {
    public:
        bool IsContinuous( vector<int> numbers ) {
            if(numbers.size() != 5)
                return false;
            int count = 0;
    		int diff = 0;
    		sort(numbers.begin(),numbers.end());
    		int i;
            for(i = 0; i < numbers.size(); ++i)
    		{
    			if(numbers[i] == 0)
    				count++;
    			else
    			{
    				break;
    			}
    		}
    		for(int j = i+1; j < numbers.size(); ++j)
    		{
    			if(numbers[j] == numbers[j-1])
    				return false;
    			else 
    				diff += numbers[j] - numbers[j-1] - 1;
    		}
    		if(count >= diff)
    			return true;
    		else
    			return false;
        }
    };
    ```

46. 孩子们的游戏(圆圈中最后剩下的数)

    每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)

    ``` cpp
    class Solution {
    public:
        int LastRemaining_Solution(int n, int m)
        {
            if(m == 0)
                return -1;
            if(n == 0)
                return 0;
            else 
                return (LastRemaining_Solution(n-1,m)+m)%n;
        }
    };
    ```

47. 求1+2+3+…+n

    求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）

    ``` cpp
    class Solution {
    public:
        int Sum_Solution(int n) {
            int ans = n;
            ans && (ans+=Sum_Solution(n-1));
            return ans;
        }
    };
    ```

48. 不用加减乘除做加法

    写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

    ``` cpp
    class Solution {
    public:
        int Add(int num1, int num2)
        {
            while(num2 != 0)
            {
                int tmp = num1^num2;
                num2 = (num1&num2)<<1;
                num1 = tmp;
            }
            return num1;
        }
    };
    ```

49. 把字符串转换成整数

    将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

    ``` cpp
    class Solution {
    public:
        int StrToInt(string str) {
            if(str.length() == 0)
                return 0;
            int flag = 0;
            bool ne = false;
            int tmp = 0;
            if(str[0] == '+')
                ne = false;
            else if(str[0] == '-')
                ne = true;
            else if(str[0] >= '0' && str[0] <= '9')
                tmp = str[0] - '0';
            else
                return 0;
            
            for(int i = 1; i < str.length(); ++i)
            {
                if(str[i] >= '0' && str[i] <= '9')
                    tmp = tmp*10 + str[i] - '0';
                else
                    return 0;
            }
            return ne ? -tmp : tmp;
        }
    };
    ```

50. 数组中重复的数字

    在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

    ``` cpp
    class Solution {
    public:
        // Parameters:
        //        numbers:     an array of integers
        //        length:      the length of array numbers
        //        duplication: (Output) the duplicated number in the array number
        // Return value:       true if the input is valid, and there are some duplications in the array number
        //                     otherwise false
        bool duplicate(int numbers[], int length, int* duplication) {
            for(int i = 0; i < length; ++i)
            {
                int index = numbers[i] >= length ? numbers[i]-length : numbers[i];
                if(numbers[index] >= length)
                {
                    *duplication = index;
                    return true;
                }
                else
                    numbers[index] += length;
            }
            return false;
        }
    };
    ```

51. 构建乘积数组

    给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

    ``` cpp
    class Solution {
    public:
        vector<int> multiply(const vector<int>& A) {
            vector<int> B(A.size(),1);
            if(A.size() < 2)
                return B;
            for(int j = 1; j < A.size(); ++j)
            {
                B[j] *= B[j-1]*A[j-1];
            }
            int tmp = 1;
            for(int j = A.size() - 2; j >= 0; --j)
            {
                tmp *= A[j+1];
                B[j] *= tmp;
            }
            
            return B;
        }
    };
    ```

52. 正则表达式匹配

    请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

    ``` cpp
    /*
    	当模式中的第二个字符不是“*”时：
    	1、如果字符串第一个字符和模式中的第一个字符相匹配，那么字符串和模式都后移一个字符，然后匹配剩余的。
    	2、如果 字符串第一个字符和模式中的第一个字符相不匹配，直接返回false。
    
    	而当模式中的第二个字符是“*”时：
    	如果字符串第一个字符跟模式第一个字符不匹配，则模式后移2个字符，继续匹配。如果字符串第一个字符跟模式第一个字符匹配，可以有3种匹配方式：
    	1、模式后移2字符，相当于x*被忽略；
    	2、字符串后移1字符，模式后移2字符；
    	3、字符串后移1字符，模式不变，即继续匹配字符下一位，因为*可以匹配多位；
    */
    
    class Solution {
    public:
        bool match(char* str, char* pattern)
        {
    		if(*str == '\0' && *pattern == '\0')//匹配到了最后返回成功
    			return true;
    		else if(*str != '\0' && *pattern == '\0')//模式到了终点，而字符还没匹配完则返回失败
    			return false;
    		else if((*pattern == *str || (*str != '\0' && *pattern == '.')) && *(pattern+1) != '*') // 匹配且 下一个不为*
    		{
    			return match(str+1,pattern+1);
    		}
    		else if((*pattern == *str || (*str != '\0' && *pattern == '.')) && *(pattern+1) == '*') // 匹配 下一个为*
    		{
    			return match(str,pattern+2) || match(str+1,pattern+2) || match(str+1,pattern);
    		}
    		else if(*pattern != *str && *(pattern+1) != '*') // 不匹配 且 下一个不为* 返回失败
    		{
    			return false;
    		}
    		else if(*pattern != *str && *(pattern+1) == '*') // 不匹配 且 下一个为*
    		{
    			return match(str,pattern+2);
    		}
            return false;
    	}
    };
    ```

53. 表示数值的字符串

    请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

    ``` cpp
    class Solution {
    public:
        bool isNumeric(char* string)
        {
    		if(*string == '\0')
    			return false;
    		
    		bool neg = false;
    		bool e = false;
    		bool point = false;
    		bool dig = false;
    		
            for(int i = 0; string[i] != '\0'; ++i)
    		{
    			if(string[i] == 'e' || string[i] == 'E')
    			{
    				if(e)//e已经出现过了
    				{
    					return false;
    				}
    				else
    				{
    					if(string[i+1] == '\0')//e后面没东西了
    						return false;
    					e = true;
    				}
    			}
    			else if(string[i] == '+' || string[i] == '-')
    			{
    				if(neg)//符号已经出现过了,这是第二次出现
    				{
    					if(string[i-1] != 'e' && string[i-1] != 'E') // 第二次出现一定跟在e的后面
    					{
    						return false;
    					}
    				}
    				else
    				{
    					neg = true;
    					if(i == 0) // 在字符串的开头
    						continue;
    					else if(string[i-1] != 'e' && string[i-1] != 'E') // 否则应该跟在e的后面
    					{
    						return false;
    					}
    				}
    			}
    			else if(string[i] == '.')
    			{
    				if(point || e)
    					return false;
    				point = true;
    			}
    			else if(string[i] >= '0' && string[i] <= '9')
    			{
    				dig = true;
    			}
                else
                    return false;
    		}
    			
    		return true;
        }
    
    };
    ```

54. 字符流中第一个不重复的字符

    请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"

    ``` cpp
    class Solution
    {
    public:
      	queue<char> q;
    	map<char,int> m;
    	//Insert one char from stringstream
        void Insert(char ch)
        {
             m[ch]++;
    		 q.push(ch);
        }
        //return the first appearence once char in current stringstream
        char FirstAppearingOnce()
        {
    		if(m[q.front()] == 1)
    			return q.front();
    		else
    		{
    			while(m[q.front()] > 1)
    				q.pop();
    			if(q.empty())
    				return '#';
    			else
    				return q.front();
    				
    		}
        }
    
    };
    ```

55. 链表中环的入口结点

    一个链表中包含环，请找出该链表的环的入口结点

    ``` cpp
    /*
    struct ListNode {
        int val;
        struct ListNode *next;
        ListNode(int x) :
            val(x), next(NULL) {
        }
    };
    */
    class Solution {
    public:
        ListNode* EntryNodeOfLoop(ListNode* pHead)
        {
    		if(pHead == NULL || pHead->next == NULL || pHead->next->next == NULL)
    			return NULL;
    		
    		ListNode *fast, *slow;
    		
    		fast = pHead->next->next;
    		slow = pHead->next;
    		while(fast != slow && fast != NULL)
    		{
    			fast = fast->next->next;
    			slow = slow->next;
    		}
            if(fast != slow)//无环
                return NULL;
    		
    		slow = pHead;
    		while(slow != fast)
    		{
    			fast = fast->next;
    			slow = slow->next;
    		}
    		return fast;
    
        }
    };
    ```

56. 删除链表中重复的结点

    在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

    ``` cpp
    /*
    struct ListNode {
        int val;
        struct ListNode *next;
        ListNode(int x) :
            val(x), next(NULL) {
        }
    };
    */
    class Solution {
    public:
        ListNode* deleteDuplication(ListNode* pHead)
        {
            if(pHead == NULL || pHead->next == NULL)
                return pHead;
    
            ListNode *first = new ListNode(pHead->val - 1);
    
            first->next = pHead;
            pHead = first;
    
            ListNode *second = first;
    
            while(second->next != NULL)
            {
                if(second->next->val != second->val && (second->next->next == NULL || second->next->val != second->next->next->val))//找到不需要删除的点
                {
                    first->next->val = second->next->val;
                    first = first->next;
                    second = second->next;
                }
                else //要么和前面相同要么和后面相同
                {
                    second = second->next;
                }
            }
            first->next = NULL;
            return pHead->next;
        }
    };
    ```

57. 二叉树的下一个结点

    给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

    ``` cpp
    /*
    struct TreeLinkNode {
        int val;
        struct TreeLinkNode *left;
        struct TreeLinkNode *right;
        struct TreeLinkNode *next;
        TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
            
        }
    };
    */
    class Solution {
    public:
        TreeLinkNode* GetNext(TreeLinkNode* pNode)
        {
    		if(pNode == NULL)
    			return NULL;
            if(pNode->right != NULL) //有右孩子，则下一个遍历的点就是 右孩子的最左孩子
    		{
    			pNode = pNode->right;
    			while(pNode->left != NULL)
    				pNode = pNode->left;
    			return pNode;
    			
    		}
    		while(pNode->next != NULL) //有父节点，则下一个点就是 第一个从左孩子 到父节点的那个父节点
    		{
    			if(pNode->next->left == pNode)
    				return pNode->next;
    			pNode = pNode->next;
    		}
    		return NULL;
        }
    };
    ```

58. 对称的二叉树

    请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

    ``` cpp
    /*
    struct TreeNode {
        int val;
        struct TreeNode *left;
        struct TreeNode *right;
        TreeNode(int x) :
                val(x), left(NULL), right(NULL) {
        }
    };
    */
    class Solution {
    public:
        bool sym(TreeNode *left, TreeNode *right)
    	{
    		if(left == NULL) return right == NULL;
    		else if(right == NULL) return false;
    		else if(left->val != right->val) return false;
    		
    		return sym(left->left,right->right) && sym(left->right,right->left);
    	}
    	
    	bool isSymmetrical(TreeNode* pRoot)
        {
    		if(pRoot == NULL)
    			return true;
    		return sym(pRoot->left,pRoot->right);
        }
    
    };
    ```

59. 按之字形顺序打印二叉树

    请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

    ``` cpp
    /*
    struct TreeNode {
        int val;
        struct TreeNode *left;
        struct TreeNode *right;
        TreeNode(int x) :
                val(x), left(NULL), right(NULL) {
        }
    };
    */
    class Solution {
    public:
        int flag = 0;
    	stack<TreeNode*> one,two;
    	
    	vector<vector<int> > Print(TreeNode* pRoot) {
    		vector<vector<int>> re;
    		if(pRoot == NULL)
    			return re;
    		flag = 1;
    		one.push(pRoot);
    		while(!one.empty() || !two.empty())
    		{
    			if(flag%2)//目前使用的是一号栈，接下来要用2号栈
    			{
    				vector<int> da;
    				while(!one.empty())
    				{
    					TreeNode *tmp = one.top();
    					one.pop();
    					da.push_back(tmp->val);
    					if(tmp->left != NULL)
    						two.push(tmp->left);
    					if(tmp->right != NULL)
    						two.push(tmp->right);
    				}
                    if(da.size())
    				    re.push_back(da);
    			}
    			else
    			{
    				vector<int> da;
    				while(!two.empty())
    				{
    					TreeNode *tmp = two.top();
    					two.pop();
    					da.push_back(tmp->val);
    					if(tmp->right != NULL)
    						one.push(tmp->right);
    					if(tmp->left != NULL)
    						one.push(tmp->left);
    				}
                    if(da.size())
    				    re.push_back(da);
    			}
    			
    			flag++;
    		}
    		return re;
        }
        
    };
    ```

60. 把二叉树打印成多行

    从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

    ``` cpp
    /*
    struct TreeNode {
        int val;
        struct TreeNode *left;
        struct TreeNode *right;
        TreeNode(int x) :
                val(x), left(NULL), right(NULL) {
        }
    };
    */
    class Solution {
    public:
        vector<vector<int> > Print(TreeNode* pRoot) {
            vector<vector<int>> re;
            if(pRoot == NULL)
                return re;
            queue<TreeNode *> p,q;
            q.push(pRoot);
            vector<int> da;
            while(!q.empty() || !p.empty())
            {
                while(!q.empty())
    			{
    				TreeNode * tmp = q.front();
    				q.pop();
                    if(tmp->left != NULL)
    				    p.push(tmp->left);
                    if(tmp->right != NULL)
    				    p.push(tmp->right);
    				da.push_back(tmp->val);
    			}
    			if(da.size())
    			{
    				re.push_back(da);
                    da.clear();
    			}
    			
    			while(!p.empty())
    			{
    				TreeNode * tmp = p.front();
    				p.pop();
                    if(tmp->left != NULL)
    				    q.push(tmp->left);
                    if(tmp->right != NULL)
    				    q.push(tmp->right);
    				da.push_back(tmp->val);
    			}
    			if(da.size())
    			{
    				re.push_back(da);
                    da.clear();
    			}
            }
    		return re;
    		
        }
        
    };
    ```

61. 序列化二叉树

    请实现两个函数，分别用来序列化和反序列化二叉树

    ``` cpp
    #include <iostream>
    #include <cstring>
    #include <cstdio>
    #include <cmath>
    
    using namespace std;
    
    struct TreeNode {
        int val;
        struct TreeNode *left;
        struct TreeNode *right;
        TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
        }
    };
    
    char* Serialize(TreeNode *root)
    {
    //    return "1,22,#10,##33,#11,##";
    
        if(root == NULL)
            return "#";
        // 一个加一是逗号，一个加一是向上取整，还有一个是空字符
        char p_m[(const int)(log10(root->val)+1+1+1)];
        // 将数字转换为字符串
        snprintf(p_m,sizeof(p_m),"%d,",root->val);
        char *p_l = Serialize(root->left);
        char *p_r = Serialize(root->right);
        char *re = (char *)malloc(strlen(p_m)+strlen(p_l)+strlen(p_r));
        re[0] = 0;
        strncat(re,p_m,strlen(p_m));
        strncat(re,p_l,strlen(p_l));
        strncat(re,p_r,strlen(p_r));
        return re;
    }
    
    TreeNode* _Deserialize(char **str)
    {
        // 一个字符都没得
        int val = 0;
        if(!(*str)[0])
            return NULL;
        else if((*str)[0] == '#')
        {
            (*str)++;
            return NULL;
        }
        else
        {
            while((*str)[0] >= '0' && (*str)[0] <= '9')
            {
                val = val*10 + (*str)[0] - '0';
                (*str)++;
            }
            if((*str)[0] == ',')
            {
                (*str)++;
                TreeNode *node = new TreeNode(val);
                node->left = _Deserialize(str);
                node->right = _Deserialize(str);
                return node;
            }
            else
            {
                (*str)++;
                return NULL;
            }
        }
    }
    
    TreeNode* Deserialize(char *str)
    {
        return _Deserialize(&str);
    }
    
    int main(int argc, char *argv[])
    {
        TreeNode *head = new TreeNode(1);
        head->left = new TreeNode(22);
        head->left->right = new TreeNode(10);
        head->right = new TreeNode(33);
        head->right->right = new TreeNode(11);
    
        char *re = Serialize(head);
        TreeNode *node = Deserialize(re);
    }
    ```

62. 二叉搜索树的第k个结点

    给定一颗二叉搜索树，请找出其中的第k大的结点。例如， 5 / \ 3 7 /\ /\ 2 4 6 8 中，按结点数值大小顺序第三个结点的值为4。

    ``` cpp
    /*
    struct TreeNode {
        int val;
        struct TreeNode *left;
        struct TreeNode *right;
        TreeNode(int x) :
                val(x), left(NULL), right(NULL) {
        }
    };
    */
    class Solution {
    public:
        TreeNode *dfs(TreeNode *pRoot, int k, int &count)
        {
            if(pRoot == NULL || k == 0)
                return NULL;
            TreeNode *ret = dfs(pRoot->left,k,count);
            if(ret == NULL)
            {
                if(k == ++count)
                    return pRoot;
                else
                    return dfs(pRoot->right,k,count);
            }
            return ret;
        }
        
        TreeNode* KthNode(TreeNode* pRoot, int k)
        {
            int count = 0;
            return dfs(pRoot, k, count);
        }
    };
    ```

63. 数据流中的中位数

    如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

    ``` cpp
    class Solution {
    public:
        vector<int> data;
        void Insert(int num)
        {
            data.push_back(num);
            for(int i = data.size()-1; i > 0; i--)
            {
                if(data[i] < data[i-1])
                    swap(data[i],data[i-1]);
            }
                
        }
    
        double GetMedian()
        { 
            if(data.size() % 2 == 0)
            {
                return (data[data.size()/2] + data[data.size()/2-1])/2.0;
            }
            else
            {
                return data[data.size()/2];
            }
        }
    
    };
    ```

64. 滑动窗口的最大值

    给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

    ``` cpp
    class Solution {
    public:
        vector<int> maxInWindows(const vector<int>& num, unsigned int size)
        {
            deque<int> de;
            vector<int> re;
            if(size == 0)
                return re;
            for(int i = 0; i < num.size(); ++i)
            {
                // 保持单调性
                while(de.size() && num[i] >= num[de.back()])
                {
                    de.pop_back();
                }
                // 维持窗口的大小
                while(de.size() && i+1-de.front() > size)
                {
                    de.pop_front();
                }
                de.push_back(i);
                if(i >= size - 1)
                    re.push_back(num[de.front()]);
            }
            return re;
        }
    };
    ```

65. 矩阵中的路径

    请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。 例如 a b c e s f c s a d e e 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

    ``` cpp
    class Solution {
    public:
        bool dfs(char* matrix,int i, int j, int rows, int cols, char* str,vector<vector<int>> &vis)
        {
            if(*str == '\0')
                return true;
            else if(matrix[i*cols+j] != *str || vis[i][j] == 1 || i == rows || j == cols || i <0 || j < 0)
                return false;
            else
            {
                vis[i][j] = 1;
                return dfs(matrix,i-1,j,rows,cols,str+1,vis) || dfs(matrix,i+1,j,rows,cols,str+1,vis) || dfs(matrix,i,j-1,rows,cols,str+1,vis) || dfs(matrix,i,j+1,rows,cols,str+1,vis);
            }
        }
    
        bool hasPath(char* matrix, int rows, int cols, char* str)
        {
            for(int start = 0; start < rows*cols; ++start)
            {
                vector<vector<int>> vis(rows,vector<int>(cols,0));
                if(dfs(matrix,start/cols,start%cols,rows,cols,str,vis))
                    return true;
            }
            return false;
        }
    };
    ```

66. 机器人的运动范围

    地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

    ``` cpp
    class Solution {
    public:
        bool goodThread(int threshold, int i, int j)
        {
            int sum = 0;
            while(i)
            {
                sum += i%10;
                i /= 10;
            }
            while(j)
            {
                sum += j%10;
                j /= 10;
            }
            if(sum > threshold)
                return false;
            else
                return true;
        }
    
        void dfs(int threshold, int rows, int cols,int i, int j, vector<vector<int>>& vis)
        {
            if(i >= 0 && i < rows && j >= 0 && j < cols && vis[i][j] == 0 &&  goodThread(threshold,i,j))
            {
                vis[i][j] = 1;
                dfs(threshold,rows,cols,i-1,j,vis);
                dfs(threshold,rows,cols,i+1,j,vis);
                dfs(threshold,rows,cols,i,j-1,vis);
                dfs(threshold,rows,cols,i,j+1,vis);
            }
            else
                return;
        }
    
        int movingCount(int threshold, int rows, int cols)
        {
            vector<vector<int>> vis(rows,vector<int>(cols,0));
            int i = 0, j = 0;
            dfs(threshold,rows,cols,i,j,vis);
            int count = 0;
            for(int i = 0; i < vis.size(); ++i)
            {
                for(int j = 0; j < vis[0].size(); ++j)
                {
                    count += vis[i][j];
                }
            }
            return count;
        }
    };
    ```

