---
title: Eajack LeetCode Notes- Day4
date: 2019-07-03 21:06:38
tags:
  - cpp
  - LeetCode
categories:
  - LeetCode
---

**PS：由于最近leetcode官网访问问题，暂改成leetcode-zh刷题。**

### Q22
#### (1) 题目信息
>* 标题：二叉树的层次遍历 II
>
>* 编号&难度：[107]，easy
>
>* Tags：tree| breadth-first-search
>
>* 描述：给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）
>
>* 例子：
>
>  ```
>  给定二叉树 [3,9,20,null,null,15,7],
>      3
>     / \
>    9  20
>      /  \
>     15   7
>  [
>    [15,7],
>    [9,20],
>    [3]
>  ]
>  ```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode.cn id=107 lang=cpp
 *
 * [107] 二叉树的层次遍历 II
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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> levelorder_nodes;
        vector<TreeNode*> temp;
        temp.push_back(root);
        int currentLevel_nodesNum = 0;

        if(root == nullptr)
    	{
    		return levelorder_nodes;
    	}

        while(temp.size())
        {
        	currentLevel_nodesNum = temp.size();
        	vector<int> currentLevelNodes_val;

        	//层次遍历
        	// 自顶向下读取，逆向储存
        	TreeNode* node_buffer = nullptr;
        	while(currentLevel_nodesNum--)
        	{
        		//pop
        		node_buffer = temp[0];
        		temp.erase(temp.begin());

        		//push
        		if(node_buffer)
        		{
        			temp.push_back(node_buffer->left);
        			temp.push_back(node_buffer->right);
        			currentLevelNodes_val.push_back(node_buffer->val);
        		}
        	}

        	//push in levelorder_nodes
        	if(levelorder_nodes.empty())
        	{
        		levelorder_nodes.push_back(currentLevelNodes_val);
        	}
        	else if(!currentLevelNodes_val.empty())
        	{
        		//levelorder_nodes右移
        		levelorder_nodes.push_back(currentLevelNodes_val);//trash
        		int last_cnt = levelorder_nodes.size()-1;
        		while(last_cnt)
        		{
        			levelorder_nodes[last_cnt] = levelorder_nodes[last_cnt-1];
        			last_cnt--;
        		}
        		levelorder_nodes[0] = currentLevelNodes_val;
        	}
        }
        
        return levelorder_nodes;
    }
};
```

>* 思路：题目要求是逆向遍历。思路依然是层次遍历思路，然而关键是逆向储存。举例如下
>
>  ```
>  给定二叉树 [3,9,20,null,null,15,7],
>      3
>     / \
>    9  20
>      /  \
>     15   7
>  levelorder_nodes = []
>  
>  第一层：遍历[3]，push => levelorder_nodes = [[3]];
>  第二层：遍历[9,20]，先右移levelorder_nodes => levelorder_nodes = [[],[3]]，注意首位为空；首部填充[9,20] => levelorder_nodes = [[9,20],[3]]
>  第三层：遍历[15,7]，先右移levelorder_nodes => levelorder_nodes = [[],[9,20],[3]]，注意首位为空；首部填充[15,7] => levelorder_nodes = [[15,7],[9,20],[3]]
>  ```

### Q23

#### (1) 题目信息

> - 标题：二叉树的层次遍历 II
>
> - 编号&难度：[107]，easy
>
> - Tags：tree| breadth-first-search
>
> - 描述：给定一个二叉树，判断它是否是高度平衡的二叉树。
>
>   本题中，一棵高度平衡二叉树定义为：
>
>   > 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过1。
>
> - 例子：
>
>   ```
>   示例1：
>   给定二叉树 [3,9,20,null,null,15,7]
>       3
>      / \
>     9  20
>       /  \
>      15   7
>   返回 true 。
>   
>   示例 2:
>   给定二叉树 [1,2,2,3,3,null,null,4,4]
>   
>          1
>         / \
>        2   2
>       / \
>      3   3
>     / \
>    4   4
>   返回 false 。
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=110 lang=cpp
 *
 * [110] 平衡二叉树
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
    int getHeight(TreeNode* root)
    {
        if(root == nullptr)
        {
            return 0;
        }
        else
        {
            int left_height = getHeight(root->left);
            int right_height = getHeight(root->right);
            return (left_height>right_height)?(left_height+1):(right_height+1);
        }
    }
public:
    bool isBalanced(TreeNode* root) {
        TreeNode* node_now = root;
        vector<TreeNode*> nodes;
        nodes.push_back(root);
        int left_h, right_h;

        while(nodes.size())
        {
            //pop
            TreeNode* node_now = nodes[0];
            nodes.erase(nodes.begin());

            //get height of left&right
            left_h = (node_now && node_now->left)?(getHeight(node_now->left)):(0);
            right_h = (node_now && node_now->right)?(getHeight(node_now->right)):(0);
            if(abs(left_h-right_h) > 1) return false;

            //push
            if(node_now)
            {
                nodes.push_back(node_now->left);
                nodes.push_back(node_now->right);
            }
        }

        return true;
    }
};
```

> - 思路：内置求节点高度的函数getHeight(TreeNode* root)。层次遍历，从第二层开始，遍历左右子树，求节点高度，if(abs(left_h-right_h) > 1) return false; 否则，继续遍历。遍历完树后，推出 return true。

### Q24

#### (1) 题目信息

> - 标题：[二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/description/)
>
> - 编号&难度：[111]，easy
>
> - Tags：[`tree`](https://leetcode.com/tag/tree) | [`depth-first-search`](https://leetcode.com/tag/depth-first-search) | [`breadth-first-search`](https://leetcode.com/tag/breadth-first-search)
>
> - 描述：给定一个二叉树，找出其最小深度。最小深度是从根节点到最近叶子节点的最短路径上的节点数量。**说明:** 叶子节点是指没有子节点的节点。
>
> - 例子：
>
>   ```
>   给定二叉树 [3,9,20,null,null,15,7],
>       3
>      / \
>     9  20
>       /  \
>      15   7
>   返回它的最小深度  2.
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=111 lang=cpp
 *
 * [111] 二叉树的最小深度
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
    int minDepth(TreeNode* root) {
	 	if(root == NULL)
			return 0;

        if(root->left == nullptr) return minDepth(root->right) + 1;
        if(root->right == nullptr) return minDepth(root->left) + 1;

        int left_h = minDepth(root->left) + 1;
        int right_h = minDepth(root->right) + 1;

        return (left_h<right_h)?(left_h):(right_h);
    }
};
```

> - 思路：这里好像记得是没想出来，看的solution。。。首先，检查根部是否有双孩子，否则的话，返回有孩子的（minDepth(root->right) + 1)；然后，求left最小深度 & right最小深度，返回二者较小者。

### Q25

#### (1) 题目信息

> - 标题：[[杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/description/)](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/description/)
>
> - 编号&难度：[118]，easy
>
> - Tags：[`array`](https://leetcode.com/tag/array)
>
> - 描述：给定一个非负整数 *numRows，*生成杨辉三角的前 *numRows* 行。
>
>   ![img](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)
>
>   在杨辉三角中，每个数是它左上方和右上方的数的和。
>
> - 例子：
>
>   ```
>   输入: 5
>   输出:
>   [
>        [1],
>       [1,1],
>      [1,2,1],
>     [1,3,3,1],
>    [1,4,6,4,1]
>   ]
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=118 lang=cpp
 *
 * [118] 杨辉三角
 */
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        if(numRows == 0)
        {
            vector<vector<int>> zero_return;            
            return zero_return;
        }

        //1- init 1&2 rows
        vector<int> row_1{1}, row_2{1,1};
        vector<vector<int>> triangle_YH;

        //2- begin
        if(numRows == 1)
        {
        	triangle_YH.push_back(row_1);
        }
        else if(numRows == 2)
        {
        	triangle_YH.push_back(row_1);
        	triangle_YH.push_back(row_2);
        }
        else
        {
        	triangle_YH.push_back(row_1);
        	triangle_YH.push_back(row_2);

        	//begin at 3-th row
        	vector<int> lastRow = row_2;
        	for(int i=3; i<=numRows; i++)
        	{
        		int cnt = i;
        		vector<int> nowRow;
        		for(int j=0; j<cnt; j++)
        		{
        			if(j==0 || j==cnt-1)
        			{
        				nowRow.push_back(1);
        			}
        			else
        			{
        				nowRow.push_back(lastRow[j-1] + lastRow[j]);
        			}
        		}
        		lastRow = nowRow;
        		triangle_YH.push_back(nowRow);
        	}
        }

        return triangle_YH;
    }
};
```

> - 思路：按照杨辉三角概念。首先，初始化好第1、2层的vector；然后，从第3层开始，保存上一层为`lastRow`，当前层初始化为空vector`nowRow`，`nowRow[0] = 1`；Next，双指针形式在`lastRow`中递进求和，push入`nowRow`，即`nowRow.push_back(lastRow[j-1] + lastRow[j])`,最后在push多1个1，即`nowRow[-1] = 1`,至此当前层`nowRow`完成，push入`triangle_YH`；继续下一层直到结束。

### Q26

#### (1) 题目信息

> - 标题：[杨辉三角 II](https://leetcode-cn.com/problems/pascals-triangle-ii/description/)
>
> - 编号&难度：[119]，easy
>
> - Tags：[`array`](https://leetcode.com/tag/array)
>
> - 描述：给定一个非负索引 *k*，其中 *k* ≤ 33，返回杨辉三角的第 *k* 行。
>
>   ![img](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)
>
>   在杨辉三角中，每个数是它左上方和右上方的数的和。
>
> - 例子：
>
>   ```
>   输入: 3
>   输出: [1,3,3,1]
>   ```
>
> * 要求：你可以优化你的算法到 *O*(*k*) 空间复杂度吗？

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=119 lang=cpp
 *
 * [119] 杨辉三角 II
 */
class Solution {
public:
    vector<int> getRow(int rowIndex) {
		vector<int> triangle_YH_k;
		int k = 0;

		//1- 0 or 1
		if(rowIndex == 0 || rowIndex == 1)
		{
			k = rowIndex + 1;
			while(k--)
			{
				triangle_YH_k.push_back(1);
			}
		}
		else//2- rowIndex >= 2
		{
			triangle_YH_k.push_back(1);
			triangle_YH_k.push_back(1);

			//begin from 2
			for(int row=2; row<=rowIndex; row++)
			{
				//每一行loop (row-1) times
				for(int cnt=1; cnt<row; cnt++)
				{
					//pop first & push (first+second)
					int value_1 = triangle_YH_k[0], value_2 = triangle_YH_k[1];
					triangle_YH_k.push_back(value_1+value_2);
					triangle_YH_k.erase(triangle_YH_k.begin());
				}
				triangle_YH_k.push_back(1);//last one
			}
		}

		return triangle_YH_k;
    }
};
```

> - 思路：其实和Q25差不多思路，只是这里只要求取第(k+1)层，且空间复杂度为O(k)。所以，`triangle_YH_k.size() = k`才行。因此，就只能逐层递进求了。可用队列形式实现，和树的层次遍历有点像：enque作为当前层node；deque首部2个上一层node求和，再enque作为当前层node。**关键：注意首部尾部1**

### Q27

#### (1) 题目信息

> - 标题：[买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/description/)
>
> - 编号&难度：[121]，easy
>
> - Tags：[`array`](https://leetcode.com/tag/array) | [`dynamic-programming`](https://leetcode.com/tag/dynamic-programming)
>
> - 描述：给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。注意你不能在买入股票前卖出股票。
>
> - 例子：
>
>   ```
>   示例 1:
>   输入: [7,1,5,3,6,4]
>   输出: 5
>   解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5。注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
>   
>   示例 2:
>   输入: [7,6,4,3,1]
>   输出: 0
>   解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=121 lang=cpp
 *
 * [121] 买卖股票的最佳时机
 */
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxIncome = INT_MIN;
        for(int i=0; i<prices.size(); i++)
        {
            int cost = prices[i];
            for(int j=i+1; j<prices.size(); j++)
            {
                int gain = prices[j];
                if(gain-cost > maxIncome)
                {
                    maxIncome = gain-cost;
                }
            }
        }

        return ( (maxIncome > 0)?maxIncome:0 );        
    }
};
```

> - 思路：因为只能产生1对数字，且顺序不能逆转。则对1个n个元素的数组，有$(n-1)+(n-2)+...+1=\frac{n^{2}+n+2}{2}=O(n^{2})$简易算法，即穷举暴力遍历。可以获得最大利润值。