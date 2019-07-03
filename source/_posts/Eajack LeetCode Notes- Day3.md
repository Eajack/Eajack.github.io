---
title: Eajack LeetCode Notes- Day3
date: 2019-06-12 21:01:38
tags:
  - cpp
  - LeetCode
categories:
  - LeetCode
---

**PS：由于最近leetcode官网访问问题，暂改成leetcode-zh刷题。**

## 1. 题目解答
### Q15
#### (1) 题目信息
>* 标题：x 的平方根
>* 编号&难度：[69]，easy
>* Tags：binary-search | math
>* 描述：实现 int sqrt(int x) 函数。计算并返回 x 的平方根，其中 x 是非负整数。由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。
>* 例子：

```
输入: 4
输出: 2

输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=69 lang=cpp
 *
 * [69] Sqrt(x)
 */
class Solution {
public:
    int mySqrt(int x) {
        long low = 0, high = x, mid = x/2;
        if(x == 1)
        {
            return 1;
        }
        else
        {
            //binary-search
            while(low < high)
            {
                if( (mid*mid == x) || ((mid*mid < x) && ((mid+1)*(mid+1) > x)) )
                {
                    return mid;
                }
                else if(mid*mid < x)
                {
                    low = mid;
                    mid = (low+high)/2;
                }
                else
                {
                    high = mid;
                    mid = (low+high)/2;
                }
            }
            return mid;
        }
        return mid;
    }
};
```

>* 思路：二分法求开方，重点在于第一个if判断`if( (mid*mid == x) || ((mid*mid < x) && ((mid+1)*(mid+1) > x)) )`。前半段`(mid*mid == x)`表明，mid刚好为sqrt数；后半段` ((mid*mid < x) && ((mid+1)*(mid+1) > x))`表明以下情况：

```
mid = 2;
x = 5;
mid*mid = 4 < 5;
(mid+1)*(mid+1) = 9 >5;
5^0.5 => 2;
```

#### (3) best solution
```cpp
//Newton method
// holy shit, so short!

/*任说1个整数x，我任猜它的平方根为y，
如果不对或精度不够准确，
那我令y = (y+x/y)/2。如此循环反复下去，y就会无限逼近x的平方根*/

long r = x;
while (r*r > x)
	r = (r + x/r) / 2;
return r;
```
>* 思路：牛顿法求开方数。。算法很简单

### Q16
#### (1) 题目信息
>* 标题：爬楼梯
>* 编号&难度：[70]，easy
>* Tags：dynamic-programming
>* 描述：假设你正在爬楼梯。需要 n 阶你才能到达楼顶。每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？注意：给定 n 是一个正整数。
>* 例子：

```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=70 lang=cpp
 *
 * [70] Climbing Stairs
 */
class Solution {
public:
    int climbStairs(int n) {
        if(n==1)
        {
            return 1;
        }
        else if(n==2)
        {
            return 2;
        }

        //Fibonacci
        int* fibonacci_array = new int[n];
        fibonacci_array[0] = 1; fibonacci_array[1] = 2;

        for(int i=2; i<n; i++)
        {
            fibonacci_array[i] = fibonacci_array[i-2] + fibonacci_array[i-1];
        }

        return fibonacci_array[n-1];
    }
};


```

>* 思路：爬楼梯问题，高中数学竞赛课碰到过的题目。。明显是**斐波那契数列**：令F(n)为爬第n级楼梯方法数，则F(n) = F(n-1) + F(n-2)，**因为爬到第n级的前一step，只有两个可能，脚在倒数第一个台阶&倒数第二个台阶。**斐波那契求解，明显不能用递归，要用顺序迭代。该题最佳思路一样，只不过不保存，没有O(n)的空间复杂度`fibonacci_array[n]`

### Q17
#### (1) 题目信息
>* 标题：删除排序链表中的重复元素
>* 编号&难度：[83]，easy
>* Tags：linked-list
>* 描述：给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
>* 例子：

```
输入: 1->1->2
输出: 1->2

输入: 1->1->2->3->3
输出: 1->2->3
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=83 lang=cpp
 *
 * [83] Remove Duplicates from Sorted List
 */
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        //len == 0 or 1
        if(head == nullptr || head->next==nullptr)
        {
            return head;
        }

        //len > 1
        ListNode *p=head, *k=head->next; 
        while(1)
        {
            int val_current = p->val;
            ListNode* temp = nullptr;
            while(k != nullptr && k->val == val_current)
            {
                temp = k;
                k = k->next;
                p->next = k;
                delete temp;
            }

            if(k == nullptr)
            {
                break;
            }
            else
            {
                p = k;
                k = p->next;
            }
            
        }

        return head;
    }
};
```

>* 思路：two-pointers思想，双指针。第二指针遍历和第一指针值对比，相等则delete掉。**双指针方法重点在于边界检查。**

#### (2) 个人Code
```cpp
class Solution {
public:
	ListNode* deleteDuplicates(ListNode* head) {
		return head && (head->next = deleteDuplicates(head->next)) && \
			head->next->val == head->val ? head->next : head;
	}
};
```

>* 思路：无fa可说。。简洁代码要多练才能想到。。

### Q18
#### (1) 题目信息
>* 标题：合并两个有序数组
>* 编号&难度：[88]，easy
>* Tags：array | two-pointers
>* 描述：给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。
说明:
(1) 初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
(2) 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。

>* 例子：

```
输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode.cn id=88 lang=cpp
 *
 * [88] 合并两个有序数组
 */
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i=m-1, j=n-1, k=m+n-1;
        while(j >= 0)
        {
            nums1[k--] = (i>=0 && nums1[i]>nums2[j])?(nums1[i--]):(nums2[j--]);
        }
    }
};
```

>* 思路：这里看了solution了。。这逆序思路很好。。


### Q19
#### (1) 题目信息
>* 标题：相同的树
>* 编号&难度：[100]，easy
>* Tags：depth-first-search | tree
>* 描述：给定两个二叉树，编写一个函数来检验它们是否相同。
如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。
>* 例子：

```
输入:       1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

输出: true

输入:      1          1
          /           \
         2             2

        [1,2],     [1,null,2]

输出: false

输入:       1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

输出: false
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode.cn id=100 lang=cpp
 *
 * [100] 相同的树
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
	bool isSameTree(TreeNode* p, TreeNode* q) {
		if( (!p&&!q) ) return true;
		if( (!p&&q) || (p&&!q) ) return false;

		return (p->val == q->val) && (isSameTree(p->left,q->left)) && \
			(isSameTree(p->right, q->right));
	}
};
```

>* 思路：巨坑在于要求中树的表示，右子树为NULL时，不表示出来。。不懂怎么弄的，看了solution，这思路很好理解。


### Q20
#### (1) 题目信息
>* 标题：对称二叉树
>* 编号&难度：[101]，easy
>* Tags：breadth-first-search | depth-first-search | tree
>* 描述：给定一个二叉树，检查它是否是镜像对称的。
>* 例子：

```
对称：
    1
   / \
  2   2
 / \ / \
3  4 4  3

不对称：
    1
   / \
  2   2
   \   \
   3    3
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode.cn id=101 lang=cpp
 *
 * [101] 对称二叉树
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(!root) return true;

        queue<TreeNode*> nodes_buffer;
        vector<TreeNode*> nodes_currentLevel;
        TreeNode* node;
        nodes_buffer.push(root);

        while(!nodes_buffer.empty())
        {
        	int buffer_size = nodes_buffer.size();
        	for(int i=0; i<buffer_size; i++)
        	{
	        	node = nodes_buffer.front();
	        	nodes_buffer.pop();
	        	nodes_currentLevel.push_back(node);

	        	if( node )
	        	{
	        		nodes_buffer.push(node->left);
	        		nodes_buffer.push(node->right);
	        	}
        	}

        	if(nodes_currentLevel.size() != 1)
        	{
        		int nodesNum = nodes_currentLevel.size();
        		for(int i=0; i<nodesNum/2; i++)
        		{
        			if(!nodes_currentLevel[i] && !nodes_currentLevel[nodesNum-1-i])
        				continue;
        			if( (!nodes_currentLevel[i]&&nodes_currentLevel[nodesNum-1-i]) || \
        				(nodes_currentLevel[i]&&!nodes_currentLevel[nodesNum-1-i]) )
        				return false;
        			
        			if(nodes_currentLevel[i]->val != nodes_currentLevel[nodesNum-1-i]->val)
        				return false;
        		}
        	}

        	nodes_currentLevel.clear();
        }

        return true;
    }
};
```

>* 思路：缓存nodes_currentLevel为树的每层节点，检查每层节点是否对称，树对称当且仅当所有层都对称，否则返回不对称。

#### (3) best solution
```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (root == NULL)
            return true;
        
        return checkSymmetric(root->left, root->right);
    }
    //check the two nodes in symmetric position
    bool checkSymmetric(TreeNode *leftSymmetricNode, TreeNode *rightSymmetricNode)
    {
        if (leftSymmetricNode == NULL && rightSymmetricNode == NULL)
            return true;
        if (leftSymmetricNode == NULL || rightSymmetricNode == NULL)
            return false;
        if (leftSymmetricNode->val == rightSymmetricNode->val)
            return checkSymmetric(leftSymmetricNode->left, rightSymmetricNode->right) && checkSymmetric(leftSymmetricNode->right, rightSymmetricNode->left);
        return false;
    }
};
```

>* 思路：**递归调用，好思路！内置函数checkSymmetric，递归调用。**重点在于`return checkSymmetric(leftSymmetricNode->left, rightSymmetricNode->right) && checkSymmetric(leftSymmetricNode->right, rightSymmetricNode->left)`。自己体会。。。

### Q21
#### (1) 题目信息
>* 标题：二叉树的最大深度
>* 编号&难度：[104]，easy
>* Tags：depth-first-search | tree
>* 描述：给定一个二叉树，找出其最大深度。二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。
>* 例子：

```
最大深度：3
   3
   / \
  9  20
    /  \
   15   7

```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode.cn id=104 lang=cpp
 *
 * [104] 二叉树的最大深度
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
	 	if(root == NULL)
			return 0;
		else
        {
            int left_height = maxDepth(root->left);
            int right_height = maxDepth(root->right);
            return 1 + ((left_height>right_height)?left_height:right_height);
        }
    }
};
```

>* 思路：《数据结构与算法分析-C语言描述》有，思路简单易懂。。