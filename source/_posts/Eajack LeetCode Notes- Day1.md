---
title: Eajack LeetCode Notes- Day1
date: 2019-05-31 23:12:38
tags:
  - cpp
  - LeetCode
categories:
  - LeetCode
---

## 1. 题目解答
### Q1
#### (1) 题目信息
>* 标题：Two Sum
>* 编号&难度：[1]，easy
>* Tags：array，hash-table
>* 描述：Given an array of integers, return indices of the two numbers such that they add up to a specific target.You may assume that each input would have exactly one solution, and you may not use the same element twice.
>* 例子：Given nums = [2, 7, 11, 15], target = 9,Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode id=1 lang=cpp
 *
 * [1] Two Sum
 */

//O(N^2)
class Solution {
public:
	vector<int> twoSum(vector<int>& nums, int target) {
		vector<int> indices;
		for(int i=0; i<nums.size(); i++)
		{
			int find_flag = 0;
			for(int j=i+1; j<nums.size(); j++)
			{
				if(nums[i]+nums[j] == target)
				{
					find_flag = 1;
					indices.push_back(i);
					indices.push_back(j);
					break;
				}
			}

			if(find_flag)
			{
				break;
			}
		}

		return indices;
	}
};
```
>* 思路：相当简单，O(N^2)，由于题目限定只考虑2个不是同1个元素的sum，因此i、j遍历

#### (3) best solution
```cpp
/* The basic idea is to maintain a hash table for each element num in nums, 
using num as key and its index (0-based) as value. 
For each num, search for target - num in the hash table. 
If it is found and is not the same element as num, then we are done.

The code is as follows. Note that each time before we add num to mp, 
we search for target - num first and so we will not hit the same element. */

//O(N)
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> indices;
        for (int i = 0; i < nums.size(); i++) {
            if (indices.find(target - nums[i]) != indices.end()) {
                return {indices[target - nums[i]], i};
            }
            indices[nums[i]] = i;
        }
        return {};
    }
};
```
>* 思路：如注释所示，引入`unordered_map`（**注意：LeetCode平台不能用map（也看情况，目前碰到的题目不能用），可以用标准库内置的unordered_map**），即哈希表。如果map中没有另一半元素，则添加`key =nums[i] , value = i`，此处`value`没用。O(N)。

### Q2
#### (1) 题目信息
>* 标题：Reverse Integer
>* 编号&难度：[7]，easy
>* Tags：math
>* 描述：Given a 32-bit signed integer, reverse digits of an integer.
>* 例子：123 => 321，-123 => -321，120 => 21
>* 备注：Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

#### (2) 个人Code
这道题没AC，弄太久了…直接看solution了

```cpp
/*
 * @lc app=leetcode id=7 lang=cpp
 *
 * [7] Reverse Integer
 */

//failed in 2019/5/30
// using solution code AC
class Solution {
public:
    int reverse(int x) {
        long result = 0;
        while(x != 0)
        {
            result = result*10 + x%10;//key code!
            x /= 10;
        }

        return ((result > INT_MAX || result < INT_MIN)?(0):(result));
};
};
```

>* 思路：可以说代码相当巧妙。。key code => `result = result*10 + x%10`，先记住吧。。还有另一种思路push & pop，类似。

```cpp
//pop operation:
pop = x % 10;
x /= 10;

//push operation:
temp = rev * 10 + pop;
rev = temp;
```

### Q3
#### (1) 题目信息
>* 标题：Palindrome Number
>* 编号&难度：[9]，easy
>* Tags：math
>* 描述：Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.
>* 备注：Coud you solve it without converting the integer to a string?

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode id=9 lang=cpp
 *
 * [9] Palindrome Number
 */
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0)
        {
            return false;
        }
        else if(x >= 0 && x < 10)
        {
            return true;
        }
        else
        {
            //reverse_x
            // using code in Problem[7]
            long reverse_x = 0, x_buffer = x;
            while(x != 0)
            {
                reverse_x = reverse_x*10 + x%10;
                x /= 10;
            }
            if(x_buffer == reverse_x)
            {
                return true;
            }
            else
            {
                return false;
            }
            
        }
    }
};
```

>* 思路：相当简单，直接沿用Q2代码，只用考虑 `x >= 10`数字（Q2代码）

#### (3) best solution

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0|| (x!=0 &&x%10==0)) return false;
        int sum=0;
        while(x>sum)
        {
            sum = sum*10+x%10;
            x = x/10;
        }
        return (x==sum)||(x==sum/10);
    }
};
```

>* 思路：差别不大，也是先剔除明显的。最后对比，因为sum省去求后连续0的和，因此补上 `(x==sum/10)`。

### Q4
#### (1) 题目信息
>* 标题：Roman to Integer
>* 编号&难度：[13]，easy
>* Tags：math，string
>* 描述：Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M. For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II. Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:（1） I can be placed before V (5) and X (10) to make 4 and 9. （2）X can be placed before L (50) and C (100) to make 40 and 90. （3）C can be placed before D (500) and M (1000) to make 400 and 900.
>* 备注：Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode id=13 lang=cpp
 *
 * [13] Roman to Integer
 */
class Solution {
public:
    int romanToInt(string s) {
        int result = 0;
        for(int i=0; i<s.size(); i++)
        {
            if(s[i] == 'I')
            {
                if(s[i+1] != 'V' && s[i+1] != 'X')
                {
                     result += 1;
                }
                else
                {
                    result += ( (s[i+1] == 'V')?(4):(9) );
                    i++;
                }
            }
            else if (s[i] == 'X')
            {
                if(s[i+1] != 'L' && s[i+1] != 'C')
                {
                     result += 10;
                }
                else
                {
                    result += ( (s[i+1] == 'L')?(40):(90) );
                    i++;
                }
            }
            else if(s[i] == 'C')
            {
                if(s[i+1] != 'D' && s[i+1] != 'M')
                {
                     result += 100;
                }
                else
                {
                    result += ( (s[i+1] == 'D')?(400):(900) );
                    i++;
                }
            }
            else if(s[i] == 'V' || s[i] == 'L')
            {
                result += ( (s[i] == 'V')?(5):(50) );
            }
            else
            {
                result += ( (s[i] == 'D')?(500):(1000) );
            }
        }

        return result;
    }
};
```

>* 思路：相当简单，先判断I、X、C，之后判断其他

#### (3) best solution

```cpp
// Points:
//  1- unordered_map cpp标准库，可以直接用，乱序map，用法与map一致
//  2- right => left, easy to judge when to add or substract

class Solution {
public:
    int romanToInt(string s) {
        if (s.empty()) { return 0; }
        unordered_map<char, int> mp { {'I', 1}, {'V', 5}, {'X', 10}, {'L', 50}, {'C', 100}, {'D', 500}, {'M', 1000} };
        int sum = mp[s.back()];
        for (int i = s.size() - 2; i >= 0; --i) {
            sum += mp[s[i]] >= mp[s[i + 1]] ? mp[s[i]] : -mp[s[i]];
        }
        return sum;
    }
};
```

>* 思路：差别不大，用了 `unordered_map`（记住！）

### Q5
#### (1) 题目信息
>* 标题：Longest Common Prefix
>* 编号&难度：[14]，easy
>* Tags：string
>* 描述：Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string "".
>* 例子：

```
Input: ["flower","flow","flight"]
Output: "fl"

Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

>* 备注：All given inputs are in lowercase letters a-z.

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode id=14 lang=cpp
 *
 * [14] Longest Common Prefix
 */

//Attention:
//  1- string内部是char字符组成
//  2- string => char： const char* p_char = p_string.c_str()
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int cnt = 0;
        string prefix = "";
        char s_i;

        //空strs
        if(strs.size() == 0)
        {
            return prefix;
        }
        
        //begin
        while(1)
        {
            if(cnt < strs[0].size())
            {
                s_i = strs[0][cnt];
            }
            else
            {
                return prefix;
            }
                        
            for(int i=0; i<strs.size(); i++)
            {
                if(cnt > strs[i].size() || strs[i][cnt] != s_i)
                {
                    return prefix;
                }
            }
            prefix += s_i;
            cnt++;
        }
    }
};


```

>* 思路：由于是前缀，因此所有string等齐扫描即可，字符初始化为strs[0][cnt]，碰到 `越界 or 不等` 即可return。

## 2. 收获&总结
最大的不足就是：**C++不熟练！！！写代码太慢了！！！**，一道题能弄个半个钟 ~ 1个钟，我真的佛了自己。。太垃圾了。。。