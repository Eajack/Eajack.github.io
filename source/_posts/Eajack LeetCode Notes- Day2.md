---
title: Eajack LeetCode Notes- Day2
date: 2019-06-06 21:57:38
tags:
  - cpp
  - LeetCode
categories:
  - LeetCode
---

## 1. 题目解答
### Q6
#### (1) 题目信息
>* 标题：Valid Parentheses
>* 编号&难度：[20]，easy
>* Tags：stack | string
>* 描述：Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
An input string is valid if:
(1) Open brackets must be closed by the same type of brackets.
(2) Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.
>* 例子：

```
Input: "()"
Output: true

Input: "()[]{}"
Output: true

Input: "(]"
Output: false

Input: "([)]"
Output: false
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=20 lang=cpp
 *
 * [20] Valid Parentheses
 */
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char, char> m = {
            {'(',')'}, {'[',']'},{'{','}'}
        };
        vector<char> stack;
        char ch;

        for(int i=0; i<s.size(); i++)
        {
            if(s[i] == '(' || s[i] == '[' || s[i] == '{')
            {
                stack.push_back(s[i]);
            }
            else
            {
                if(stack.size() != 0)
                {
                    ch = stack.back();
                    stack.pop_back();
                    if(s[i] != m[ch])
                    {
                        return false;
                    }
                }
                else
                {
                    return false;
                }
            }
        }

        return stack.empty();
    }
};
```

>* 思路：这题目在《数据结构与算法分析-C语言描述》中的“栈的应用”章节有提及。**思路：遍历所有字符，碰到左边符A（包括：(、{、[），push入栈；碰到右边符号B（包括：)、}、]），则pop出栈中元素E，若E不是B对应的右边符，return false，否则继续遍历。当便利完成，检查是否空栈，空栈则返回true，否则返回false。**

#### (3) best solution
```cpp
/*Repetitive code but I guess this is clean, and easy to understand. 
This solution also accepts (and ignores) any characters other than parenthesis in the string. 
Hence, it can be used to check if the parenthesis matches in an equation for example.*/


#include <stack>

class Solution {
public:
    bool isValid(string s) {
        stack<char> paren;
        for (char& c : s) {
            switch (c) {
                case '(': 
                case '{': 
                case '[': paren.push(c); break;
                case ')': if (paren.empty() || paren.top()!='(') return false; else paren.pop(); break;
                case '}': if (paren.empty() || paren.top()!='{') return false; else paren.pop(); break;
                case ']': if (paren.empty() || paren.top()!='[') return false; else paren.pop(); break;
                default: ; // pass
            }
        }
        return paren.empty() ;
    }
};
```
>* 思路：思路其实一致的，**需要学习的点：case判断char字符；写得真优雅！排版太好看了！**

### Q7
#### (1) 题目信息
>* 标题：Merge Two Sorted Lists
>* 编号&难度：[21]，easy
>* Tags：linked-list
>* 描述：Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.
>* 例子：

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=21 lang=cpp
 *
 * [21] Merge Two Sorted Lists
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        //0-
        if(l1 == nullptr || l2 == nullptr)
        {
            return (l1 == nullptr)?(l2):(l1);
        }
        ListNode* l3 = (l1->val <= l2->val)?(l1):(l2);
        ListNode* l4 = (l1->val <= l2->val)?(l2):(l1);

        //1- 
        ListNode *p2 = l4, *temp1 = nullptr, *temp2 = nullptr;
        while(p2 != nullptr)
        {
            int p2_now_val = p2->val;
            bool allPass_flag = true;
            ListNode *p1_now = l3;
            while(p1_now != nullptr)
            {
                int p1_next_val = (p1_now->next == nullptr)?(INT_MAX):(p1_now->next->val);
                if(p1_now->val <= p2_now_val && p2_now_val <= p1_next_val)
                {
                    temp1 = p1_now->next;
                    p1_now->next = p2;
                    temp2 = p2->next;
                    p2->next = temp1;
                    p2 = temp2;

                    allPass_flag = false;
                    break;
                }
                p1_now = p1_now->next;
            }

            if(allPass_flag)
            {
                l4->next = l3;
                return l4;
            }
        }

        return l3;
    }
};
```

>* 思路：伪代码如下（并不规范）

```
BEGIN:
	if(l1 == nullptr or l2 == nullptr):
		return (l1 == nullptr)?(l2):(l1);

	l3 = l1&l2中首元素较小者; l4 = l1&l2中首元素较大者;

	for l4node in l4_nodes:
		val_current = l4node->val;
		val_next = next_node->val;
		for l3node in l3_nodes:
			val_l3node_now = l3node->val;
			if(val_current <= val_l3node_now <= val_next):
				insert l3node between l4node & next_node
END
```

#### (3) best solution
```cpp
/*C++

The first line ensures that a is at least as good a list head as b, 
by swapping them if that’s not already the case. 
The second line merges the remaining lists behind a.*/
ListNode* mergeTwoLists(ListNode* a, ListNode* b) {
    if (!a || b && a->val > b->val) swap(a, b);
    if (a) a->next = mergeTwoLists(a->next, b);
    return a;
}

/*C

Same solution, I just have to replace C++'s swap. 
I’m not sure whether evaluation order is standardized, 
but it worked and got accepted this way.*/
struct ListNode* mergeTwoLists(struct ListNode* a, struct ListNode* b) {
    if (!a || b && a->val > b->val) a += b - (b = a);
    if (a) a->next = mergeTwoLists(a->next, b);
    return a;
}
```
>* 思路：这个思路类似归并排序（merge-sort），但真的巧妙了，慢慢学习吧。。先记住。

### Q8
#### (1) 题目信息
>* 标题：Remove Duplicates from Sorted Array
>* 编号&难度：[26]，easy
>* Tags：array | two-pointers
>* 描述：Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.
Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.
>* 例子：

```
Given nums = [1,1,2],
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the returned length.

Given nums = [0,0,1,1,1,2,2,3,3,4],
Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.
It doesn't matter what values are set beyond the returned length.
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=26 lang=cpp
 *
 * [26] Remove Duplicates from Sorted Array
 */
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        //0- 0&1 check
        if(nums.empty() || nums.size() == 1)
        {
            return nums.size();
        }

        //1- begin
        int i = 0, j = 1, cnt = 0;
        while(i < nums.size()-1 && j <= nums.size()-1)
        {
            while (j <= nums.size()-1)
            {
                if(nums[j] == nums[i])
                {
                    j++;
                }
                else
                {
                    nums[cnt] = nums[i];
                    i = j;
                    j = i+1;
                    cnt++;
                    break;
                }
            }
        }
        nums[cnt] = nums[i];

        return (cnt+1);
    }
};
```

>* 思路：two-pointers思想，双指针

```
BEGIN
	if(nums 为空 or nums 长度为1):
		return nums.size()

	int i=0, j=1 //双指针
	int cnt=0 //更新后数组索引
	while(i < nums.size()-1 and j <= nums.size()-1):#保证双指针均不越界
		while (j <= nums.size()-1):	#保证j指针不越界，以j指针遍历
			if(nums[j] == nums[i]):
				j++
			else:
				nums[cnt]更新，指针i=j, j=i+1, 索引cnt++
				break

	nums[cnt] = nums[i];

	return(cnt+1)
END
```

### Q9
#### (1) 题目信息
>* 标题：Remove Element
>* 编号&难度：[27]，easy
>* Tags：array | two-pointers
>* 描述：Given an array nums and a value val, remove all instances of that value in-place and return the new length.
Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.
The order of elements can be changed. It doesn't matter what you leave beyond the new length.
>* 例子：

```
Given nums = [3,2,2,3], val = 3,
Your function should return length = 2, with the first two elements of nums being 2.
It doesn't matter what you leave beyond the returned length.

Given nums = [0,1,2,2,3,0,4,2], val = 2,
Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.
Note that the order of those five elements can be arbitrary.
It doesn't matter what values are set beyond the returned length.
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=27 lang=cpp
 *
 * [27] Remove Element
 */
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int new_index = 0;
        for(int i=0; i<nums.size(); i++)
        {
            if(nums[i] != val)
            {
                nums[new_index] = nums[i];
                new_index++;
            }
        }
        return new_index;
    }
};
```

>* 思路：two-pointers思想，双指针。思路其实很简单，然而最终还是要看答案。。** 即初始化new_index = 0，遍历输入数组，若当前值 != val，则nums[new_index] = nums[i]，new_index++ **

### Q10
#### (1) 题目信息
>* 标题：Implement strStr
>* 编号&难度：[27]，easy
>* Tags：string | two-pointers
>* 描述：Implement strStr().
Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
>* 例子：

```
Input: haystack = "hello", needle = "ll"
Output: 2

Input: haystack = "aaaaa", needle = "bba"
Output: -1

For the purpose of this problem, we will return 0 when needle is an empty string
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=28 lang=cpp
 *
 * [28] Implement strStr()
 */
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(needle.size() == 0)
        {
            return 0;
        }

        int find_index = -1;
        for(int i=0; i<haystack.size(); i++)
        {
            int same_cnt = 0;
            if(haystack[i] == needle[0])
            {
                find_index = i;
                same_cnt++;

                // haystack 不包含 needle
                if(haystack.size()-i+1 < needle.size())
                {
                    return -1;
                }

                // haystack 包含 needle长度，遍历
                for(int j=i+1, k=1; k<needle.size(); j++, k++)
                {
                    if(haystack[j] == needle[k])
                    {
                        same_cnt++;
                    }
                }

                if(same_cnt == needle.size())
                {
                    return find_index;
                }
            }
        }

        return -1;
    }
};
```

>* 思路：首先，遍历haystack字符串，寻找和needle字符串首个char相同的位置i， 记录find_index = i；然后，判断剩余的haystack字符串能否覆盖needle字符串，若不能，则return false，否则遍历同时遍历 haystack字符串 & needle字符串，计算相同个数same_cnt；最后，判断same_cnt == needle.size()，return find_index，否则return -1。

#### (3) best solution
```cpp
// brute
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.size(), n = needle.size();
        for (int i = 0; i <= m - n; i++) {
            int j = 0;
            for (; j < n; j++) {
                if (haystack[i + j] != needle[j]) {
                    break;
                }
            }
            if (j == n) {
                return i;
            }
        }
        return -1;
    }
};

//KMP
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.size(), n = needle.size();
        if (!n) {
            return 0;
        }
        vector<int> lps = kmpProcess(needle);
        for (int i = 0, j = 0; i < m;) {
            if (haystack[i] == needle[j]) { 
                i++, j++;
            }
            if (j == n) {
                return i - j;
            }
            if (i < m && haystack[i] != needle[j]) {
                j ? j = lps[j - 1] : i++;
            }
        }
        return -1;
    }
private:
    vector<int> kmpProcess(string needle) {
        int n = needle.size();
        vector<int> lps(n, 0);
        for (int i = 1, len = 0; i < n;) {
            if (needle[i] == needle[len]) {
                lps[i++] = ++len;
            } else if (len) {
                len = lps[len - 1];
            } else {
                lps[i++] = 0;
            }
        }
        return lps;
    }
};
```

>* 思路：brute暴力搜索类似我的解决方案，但更简略；KMP是有名的字符串搜索算法；

### Q11
#### (1) 题目信息
>* 标题：Search Insert Position
>* 编号&难度：[35]，easy
>* Tags：array | binary-search
>* 描述：Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
You may assume no duplicates in the array.
>* 例子：

```
Input: [1,3,5,6], 5
Output: 2

Input: [1,3,5,6], 2
Output: 1

Input: [1,3,5,6], 7
Output: 4

Input: [1,3,5,6], 0
Output: 0
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=35 lang=cpp
 *
 * [35] Search Insert Position
 */
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int low = 0, high = nums.size()-1, mid = 0;
        while(low <= high)
        {
            mid = (low+high)/2;
            if(nums[mid] < target)
            {
                low = mid+1;
            }
            else if(nums[mid] > target)
            {
                high = mid-1;
            }
            else
            {
                return mid;
            }
        }
        
        //not found
        return (low>high)?(low):(high);
    }
};
```

>* 思路：这题就是binary-search变种，找返回位置，否则返回low & high较大者。

### Q12
#### (1) 题目信息
>* 标题：Maximum Subarray
>* 编号&难度：[53]，easy
>* Tags：array | divide-and-conquer | dynamic-programming
>* 描述：Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
>* 例子：

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=53 lang=cpp
 *
 * [53] Maximum Subarray
 */
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int thisSum, maxSum, i;
        thisSum = maxSum = 0;
        for(int i=0; i<nums.size(); i++)
        {
            thisSum += nums[i];
            if(thisSum > maxSum)
            {
                maxSum = thisSum;
            }
            else if(thisSum < 0)
            {
                thisSum = 0;
            }
        }

        //all are minus
        if(maxSum == 0)
        {
            maxSum = nums[0];
            for(int i=1; i<nums.size(); i++)
            {
                maxSum = (nums[i]>maxSum)?(nums[i]):(maxSum);
            }
        }

        return maxSum;
    }
};
```

>* 思路：最大子序列和，在《数据结构与算法分析-C语言描述》中有，此处也是用的书中代码改一下。思路很巧妙：**除了全负数序列外，其他序列的最大子序列和一定大于等于0**，因此设置maxSum = 0 ，遍历N次逐项累加thisSum，若thisSum > maxSum，则更新maxSum，若thisSum < 0，则令thisSum = 0继续遍历。遍历完后，判断maxSum == 0，若是则证明为全负数序列，取序列最大值return；否则return 原来的maxSum（> 0）。**best solution可以用分治，这里就不说了。。**

### Q13
#### (1) 题目信息
>* 标题：Length of Last Word
>* 编号&难度：[58]，easy
>* Tags：string
>* 描述：Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.
If the last word does not exist, return 0.
Note: A word is defined as a character sequence consists of non-space characters only.
>* 例子：

```
Input: "Hello World"
Output: 5
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=58 lang=cpp
 *
 * [58] Length of Last Word
 */
class Solution {
public:
    int lengthOfLastWord(string s) {
        int last_blank_index = -1, word_len = 0;
        int s_begin = 0, s_end = s.size()-1;
        
        //1- cut head & tail blanks(s.strip())
        while(s_begin < s.size() && s[s_begin] == ' ')
        {
            s_begin++;
        }
        while(s_end >= 0 && s[s_end] == ' ')
        {
            s_end--;
        }

        //2- check if s.strip() == ''
        if(s_begin == s.size() && s_end == -1)
        {
            return 0;
        }

        //3- begin
        for(int i=s_begin; i<s_end+1; i++)//get the last_blank_index
        {
            if(s[i] == ' ')
            {
                last_blank_index = i;
            }
        }

        if(last_blank_index != -1)//has blank inside s.strip()
        {
            for(int i=last_blank_index+1; i<s_end+1; i++)
            {
                word_len++;
            }
        }
        else//no blank inside s.strip() && s.strip() != ''
        {
            word_len = s_end-s_begin+1;
        }
        

        return word_len;
    }
};
```

>* 思路：1- s.strip()（类似python函数）；2- 判断s.strip() == ''，是则返回0；3-开始遍历获得最后一个空格，对应的后面word，计算len。

#### (3) best solution
```cpp
/* Well, the basic idea is very simple. 
Start from the tail of s and move backwards to find the first non-space character. 
Then from this character, move backwards and count the number of non-space characters 
until we pass over the head of s or meet a space character. 
The count will then be the length of the last word. */

//从尾部开始找第一个非空格字符，开始len++，直到倒退找到第一个空格字符

class Solution {
public:
    int lengthOfLastWord(string s) { 
        int len = 0, tail = s.length() - 1;
        while (tail >= 0 && s[tail] == ' ') tail--;
        while (tail >= 0 && s[tail] != ' ') {
            len++;
            tail--;
        }
        return len;
    }
};
```

>* 思路：如注释。。**逆向思维，这才是好解答。**

### Q14
#### (1) 题目信息
>* 标题：Add Binary
>* 编号&难度：[67]，easy
>* Tags：math | string
>* 描述：Given two binary strings, return their sum (also a binary string).
The input strings are both non-empty and contains only characters 1 or 0.
>* 例子：

```
Input: a = "11", b = "1"
Output: "100"

Input: a = "1010", b = "1011"
Output: "10101"
```

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=67 lang=cpp
 *
 * [67] Add Binary
 */
class Solution {
public:
    string addBinary(string a, string b) {
        string ab_sum, ab_sum_return;
        char sum_now = '0', carry = '0';
        bool a_bool, b_bool, carry_bool;

        //1- 补0等长 + 1
        int a_len = a.size(), b_len = b.size();
        if(a_len != b_len)
        {
            int extraLen = abs(a_len-b_len)+1;
            bool aorb = (a_len < b_len);

            for(int i=1; i<=extraLen; i++)
            {
                if(aorb)
                {
                    a = '0' + a;
                    if(i==1) b = '0' + b;
                }
                else
                {
                    b = '0' + b;
                    if(i==1) a = '0' + a;
                }
            }
        }

        //2- begin
        for(int i=a.size()-1; i>=0; i--)
        {
            a_bool = (a[i] == '1')?(true):(false);
            b_bool = (b[i] == '1')?(true):(false);
            carry_bool = (carry == '1')?(true):(false);

            //真值表
            sum_now = ( ((!a_bool)&&(!b_bool)&&(carry_bool)) || ((!a_bool)&&(b_bool)&&(!carry_bool)) || \
                ((a_bool)&&(!b_bool)&&(!carry_bool)) || ((a_bool)&&(b_bool)&&(carry_bool)) )? ('1'):('0');
            carry = ( ((!a_bool)&&(b_bool)&&(carry_bool)) || ((a_bool)&&(!b_bool)&&(carry_bool)) || \
                ((a_bool)&&(b_bool)&&(!carry_bool)) || ((a_bool)&&(b_bool)&&(carry_bool)) )? ('1'):('0');
            ab_sum = sum_now + ab_sum;
        }
        ab_sum = carry + ab_sum;

        //3- ab_sum.lstrip()
        int first_notZero_index = 0;
        while(first_notZero_index < ab_sum.size()-1 && ab_sum[first_notZero_index] == '0')
        {
            first_notZero_index++;
        }
        if (first_notZero_index == -1)
        {
            first_notZero_index = 1;
        }

        for(int i=first_notZero_index; i<ab_sum.size(); i++)
        {
            ab_sum_return += ab_sum[i];
        }

        return ab_sum_return;
    }
};
```

>* 思路：二进制加法，个人代码采用典型的**数字电路的真值表解法**，我觉得这做法比best solution好。代码不难懂，需要一些基本数电基础即可。