---
title: Eajack LeetCode Notes- Day6
date: 2019-08-12 21:06:38
tags:
  - cpp
  - LeetCode
categories:
  - LeetCode

---

### Q45

#### (1) 题目信息
>* 标题：[用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/description/)
>
>* 编号&难度：[225]，easy
>
>* Tags：[`stack`](https://leetcode.com/tag/stack) | [`design`](https://leetcode.com/tag/design)
>
>* 描述：使用队列实现栈的下列操作：
>
>  - push(x) -- 元素 x 入栈
>  - pop() -- 移除栈顶元素
>  - top() -- 获取栈顶元素
>  - empty() -- 返回栈是否为空
>
>  **注意:**
>
>  - 你只能使用队列的基本操作-- 也就是 `push to back`, `peek/pop from front`, `size`, 和 `is empty` 这些操作是合法的。
>  - 你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
>  - 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。
>

#### (2) 个人Code
```cpp
/*
 * @lc app=leetcode.cn id=225 lang=cpp
 *
 * [225] 用队列实现栈
 */
class MyStack {
    queue<int> q1, q2;
public:
    /** Initialize your data structure here. */
    MyStack() {
        
    }
    
    /** Push element x onto stack. */
    void push(int x) {
        q1.push(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        //首先pop出到q2，直到只剩下q1栈顶
        while(q1.size() > 1)
        {
            q2.push(q1.front());
            q1.pop();
        }
        //push逆向
        int topNum = q1.front();
        q1.pop();
        while(q2.size() > 0)
        {
            q1.push(q2.front());
            q2.pop();
        }

        return topNum;
    }
    
    /** Get the top element. */
    int top() {
        //首先pop出到q2，直到只剩下q1栈顶
        while(q1.size() > 1)
        {
            q2.push(q1.front());
            q1.pop();
        }
        //push逆向
        int topNum = q1.front();
        q1.pop();
        q2.push(topNum);
        while(q2.size() > 0)
        {
            q1.push(q2.front());
            q2.pop();
        }

        return topNum;
    }
    
    /** Returns whether the stack is empty. */
    bool empty() {
        return (q1.empty());
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

>* 思路：2个queue实现1个stack，1个queue用来缓存pop过程出来的元素，另一个栈直接储存push进来的元素
>

### Q46

#### (1) 题目信息

> - 标题：[[翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/description/)](https://leetcode-cn.com/problems/implement-stack-using-queues/description/)
>
> - 编号&难度：[226]，easy
>
> - Tags：[`tree`](https://leetcode.com/tag/tree)
>
> - 描述：
>
>   输入：
>
>   ```
>        4
>      /   \
>     2     7
>    / \   / \
>   1   3 6   9
>   ```
>
>   输出：
>
>   ```
>        4
>      /   \
>     7     2
>    / \   / \
>   9   6 3   1
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=226 lang=cpp
 *
 * [226] 翻转二叉树
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
    TreeNode* invertTree(TreeNode* root) {
        if(!root || (!root->left && !root->right)){
        	return root;
        }
        else{
        	TreeNode* temp = root->left;
        	root->left = root->right;
        	root->right = temp;
        	if(root->left)
        		TreeNode *trash1 = invertTree(root->left);
        	if(root->right)
        		TreeNode *trash2 = invertTree(root->right);
        }
        return root;
    }
};
```

> - 思路：首先根部左右子树交换，然后递归地对左右子树调用原函数

### Q47

#### (1) 题目信息

> - 标题：[2的幂](https://leetcode-cn.com/problems/power-of-two/description/)
> - 编号&难度：[226]，easy
> - Tags：[`math`](https://leetcode.com/tag/math) | [`bit-manipulation`](https://leetcode.com/tag/bit-manipulation)
> - 描述：给定一个整数，编写一个函数来判断它是否是 2 的幂次方。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=231 lang=cpp
 *
 * [231] 2的幂
 */
class Solution {
public:
    bool isPowerOfTwo(int n) {
        //注意：n&(n-1) == 0是错的, 应该是(n&(n-1)) == 0
        return ( (n>0) && !(n&(n-1)));
    }
};
```

> - 思路：看了solution，检查二进制表示的首位digit是否为1即可

### Q48

#### (1) 题目信息

> - 标题：[用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/description/)
> - 编号&难度：[226]，easy
> - Tags：[`stack`](https://leetcode.com/tag/stack) | [`design`](https://leetcode.com/tag/design)
> - 描述：使用栈实现队列的下列操作：
>   - push(x) -- 将一个元素放入队列的尾部。
>   - pop() -- 从队列首部移除元素。
>   - peek() -- 返回队列首部的元素。
>   - empty() -- 返回队列是否为空。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=232 lang=cpp
 *
 * [232] 用栈实现队列
 */
class MyQueue {
public:
    vector<int> s1, s2; 
    /** Initialize your data structure here. */
    MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        s1.push_back(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        while(s1.size() != 1){
            s2.push_back(s1.back());
            s1.pop_back();           
        }

        int top_val = s1.front();
        s1.pop_back();
        while(!s2.empty()){
            s1.push_back(s2.back());
            s2.pop_back();
        }

        return top_val;
    }
    
    /** Get the front element. */
    int peek() {
        while(s1.size() != 1){
            s2.push_back(s1.back());
            s1.pop_back();           
        }
        int top_val = s1.front();
        while(!s2.empty()){
            s1.push_back(s2.back());
            s2.pop_back();
        }

        return top_val;
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return s1.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

> - 思路：和“用队列实现栈”差不多。这里用2个栈实现队列，其中1个栈用push代替队列的enqueue，另一个栈作为缓存区辅助dequeue

### Q49

#### (1) 题目信息

> - 标题：[回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/description/)
> - 编号&难度：[226]，easy
> - Tags：[`linked-list`](https://leetcode.com/tag/linked-list) | [`two-pointers`](https://leetcode.com/tag/two-pointers)
> - 描述：请判断一个链表是否为回文链表。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=234 lang=cpp
 *
 * [234] 回文链表
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
    bool returnFlag = false;

    void isHeadEqualTail(ListNode* head, int len) {
        if(len <= 1) {
            returnFlag = true;
        }
        else {
            ListNode* tail = head;
            int cnt = len - 1;
            while(cnt--) tail = tail->next;
            if(head->val != tail->val) {
                returnFlag = false;
            }
            else {
                isHeadEqualTail(head->next, len-2);
            }
        }
    }

    bool isPalindrome(ListNode* head) {
        ListNode* temp = head;
        int len = 0;
        while (temp) {
            ++len;
            temp = temp->next;
        }

        if(len <= 1)
        {
            return true;
        }
        else
        {
            isHeadEqualTail(head, len);
        }

        return returnFlag;
    }
};
```

> - 思路：最简单思路是首位部双指针，这代码思路是：递归判断首位val是否相等，通过链表长和表头变化递归

### Q50

#### (1) 题目信息

> - 标题：[删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/description/)
> - 编号&难度：[237]，easy
> - Tags：[`linked-list`](https://leetcode.com/tag/linked-list)
> - 描述：请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点，你将只被给定要求被删除的节点。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=237 lang=cpp
 *
 * [237] 删除链表中的节点
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
    void deleteNode(ListNode* node) {
        if(node == nullptr) return;

        ListNode* temp = node->next;
        node->val = temp->val;
        node->next = temp->next;

        delete temp;
    }
};
```

> - 思路：看了solution，只能说这个巧妙。。并没有传表头进来。**因为不是首位部node，所以可以复制下一个到当前位，然后删除下一个node。**

### Q51

#### (1) 题目信息

> - 标题：[有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/description/)
>
> - 编号&难度：[242]，easy
>
> - Tags：[`hash-table`](https://leetcode.com/tag/hash-table) | [`sort`](https://leetcode.com/tag/sort)
>
> - 描述：给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。
>
> - 例子：
>
>   **示例 1:**
>
>   ```
>   输入: s = "anagram", t = "nagaram"
>   输出: true
>   ```
>
>   **示例 2:**
>
>   ```
>   输入: s = "rat", t = "car"
>   输出: false
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=242 lang=cpp
 *
 * [242] 有效的字母异位词
 */
class Solution {
public:
    bool isAnagram(string s, string t) {
        if((s.length() || t.length()) && (s.length() != t.length())){
            return false;
        }
        else if(s.length() == 0 && t.length() == 0){
            return true;
        }

        unordered_map<char, int> s_num, t_num;
        for(char ch:s){
            if(s_num.find(ch) == s_num.end()){
                s_num[ch] = 1;
            }
            else{
                s_num[ch]++;
            }
        }
        for(char ch:t){
            if(t_num.find(ch) == t_num.end()){
                t_num[ch] = 1;
            }
            else{
                t_num[ch]++;
            }
        }

        for(auto iter=s_num.begin(); iter != s_num.end(); iter++){
           if(t_num[iter->first] != iter->second){
               return false;
           }
        }

        return true;
    }
};
```

> - 思路：看了solution。solution给出2种方案：**（1）统计char出现个数，二者对应相等即可（2）sort后的2个string相等**

### Q52

#### (1) 题目信息

> - 标题：[二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/description/)
>
> - 编号&难度：[252]，easy
>
> - Tags：[`tree`](https://leetcode.com/tag/tree) | [`depth-first-search`](https://leetcode.com/tag/depth-first-search)
>
> - 描述：给定一个二叉树，返回所有从根节点到叶子节点的路径。
>
>   **说明:** 叶子节点是指没有子节点的节点。
>
> - 例子：
>
>   ```
>   输入:
>   
>      1
>    /   \
>   2     3
>    \
>     5
>   
>   输出: ["1->2->5", "1->3"]
>   
>   解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=257 lang=cpp
 *
 * [257] 二叉树的所有路径
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
    void getPath(TreeNode* node, vector<string>& paths, string path){
        if(!node->left && !node->right){
            paths.push_back(path);
            return;
        }

        if(node->left){
            getPath(node->left, paths, path+"->"+to_string(node->left->val));
        }

        if(node->right){
            getPath(node->right, paths, path+"->"+to_string(node->right->val));
        }
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> paths;
        if(root){
            getPath(root, paths, to_string(root->val));
        }

        return paths;
    }
};
```

> - 思路：看了solution，记住，深度遍历

### Q53

#### (1) 题目信息

> - 标题：[各位相加](https://leetcode-cn.com/problems/add-digits/description/)
>
> - 编号&难度：[258]，easy
>
> - Tags：[`math`](https://leetcode.com/tag/math)
>
> - 描述：给定一个非负整数 `num`，反复将各个位上的数字相加，直到结果为一位数。
>
> - 例子：
>
>   ```
>   输入: 38
>   输出: 2 
>   解释: 各位相加的过程为：3 + 8 = 11, 1 + 1 = 2。 由于 2 是一位数，所以返回 2。
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=258 lang=cpp
 *
 * [258] 各位相加
 */
class Solution {
public:
    int addDigits(int num) {
        int sum;

        do{
            sum = 0;
            vector<int> digits;
            while(num/10){
                digits.push_back(num%10);
                num /= 10;
            }
            digits.push_back(num);
            for(int digit:digits){
                sum += digit;
            }
            num = sum;
        }
        while(num >= 10);

        return num;
    }
};
```

> - 思路：提取各位数字求和相加

### Q54

#### (1) 题目信息

> - 标题：[移动零](https://leetcode-cn.com/problems/move-zeroes/description/)
>
> - 编号&难度：[283]，easy
>
> - Tags：[`array`](https://leetcode.com/tag/array) | [`two-pointers`](https://leetcode.com/tag/two-pointers)
>
> - 描述：给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> - 例子：
>
>   ```
>   输入: [0,1,0,3,12]
>   输出: [1,3,12,0,0]
>   ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=283 lang=cpp
 *
 * [283] 移动零
 */
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int temp, j;
        for(int i=0; i<nums.size(); i++){
            if(nums[i] == 0){
                j = i+1;
                while(1){
                    if(nums[i] == 0 && j < nums.size()){
                        temp = nums[i];
                        nums[i] = nums[j];
                        nums[j] = temp;
                        j++;
                    }
                    else{
                        break;
                    }
                }

                if(j >= nums.size()){
                    break;
                }
            }
        }
    }
};
```

> - 思路：遍历逐位右移，solution思路更好：优先把非0值前移，最后后面至0

### Q55

#### (1) 题目信息

> - 标题：[单词规律](https://leetcode-cn.com/problems/word-pattern/description/)
> - 编号&难度：[290]，easy
> - Tags：[`hash-table`](https://leetcode.com/tag/hash-table)
> - 描述：给定一种规律 `pattern` 和一个字符串 `str` ，判断 `str` 是否遵循相同的规律。这里的 **遵循** 指完全匹配，例如， `pattern` 里的每个字母和字符串 `str` 中的每个非空单词之间存在着双向连接的对应规律。
> - 例子：
>
> **示例1:**
>
> ```
> 输入: pattern = "abba", str = "dog cat cat dog"
> 输出: true
> ```
>
> **示例 2:**
>
> ```
> 输入:pattern = "abba", str = "dog cat cat fish"
> 输出: false
> ```
>
> **示例 3:**
>
> ```
> 输入: pattern = "aaaa", str = "dog cat cat dog"
> 输出: false
> ```
>
> **示例 4:**
>
> ```
> 输入: pattern = "abba", str = "dog dog dog dog"
> 输出: false
> ```

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=290 lang=cpp
 *
 * [290] 单词规律
 */
class Solution {
public:
    vector<string> split(string str){
        vector<string> str_split;
        string split_one;
        for(int i=0; i<=str.size(); i++){
            if(str[i] == ' ' || i == str.size()){
                str_split.push_back(split_one);
                split_one = "";
            }
            else{
                split_one += str[i];
            }
        }

        return str_split;
    }

    bool wordPattern(string pattern, string str) {
        vector<string> str_split = split(str);

        if(str_split.size() != pattern.length()){
            return false;
        }

        unordered_map<char, string> table1;
        for(int i=0; i<pattern.size(); i++){
            if(table1.find(pattern[i]) == table1.end()){
                for(auto iter = table1.begin(); iter != table1.end();iter++){
                    if(iter->second == str_split[i]){
                        return false;
                    }
                }

                table1[pattern[i]] = str_split[i];
            }
            else{
                if(table1[pattern[i]] != str_split[i]){
                    return false;
                }
            }
        }

        return true;
    }
};
```

> - 思路：字符串split + 双向map检查

### Q56

#### (1) 题目信息

> - 标题：[3的幂](https://leetcode-cn.com/problems/power-of-three/description/)
> - 编号&难度：[326]，easy
> - Tags：[`math`](https://leetcode.com/tag/math)
> - 描述：给定一个整数，写一个函数来判断它是否是 3 /n的幂次方。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=326 lang=cpp
 *
 * [326] 3的幂
 */
class Solution {
public:
    string dec2Ndigit(int num, int n){
        string num_digit = "";
        while(num/n){
            num_digit = to_string(num%n) + num_digit;
            num /= n;
        }
        num_digit = to_string(num) + num_digit;

        return num_digit;
    }

    bool isPowerOfThree(int n) {
        string n_digit = dec2Ndigit(n, 3);

        int sum = 0;
        for(char ch:n_digit){
            sum += (ch-'0');
        }

        if(sum == 1 && n_digit[0] == '1'){
            return true;
        }

        return false;
    }
};
```

> - 思路：和2的幂思路一样，区别在于3进制。其实n的幂思路一致，只是转成n进制表示即可

### Q57

#### (1) 题目信息

> - 标题：[反转字符串](https://leetcode-cn.com/problems/reverse-string/description/)
> - 编号&难度：[344]，easy
> - Tags：[`two-pointers`](https://leetcode.com/tag/two-pointers) | [`string`](https://leetcode.com/tag/string)
> - 描述：编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `char[]` 的形式给出。不要给另外的数组分配额外的空间，你必须**原地修改输入数组**、使用 O(1) 的额外空间解决这一问题。你可以假设数组中的所有字符都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=344 lang=cpp
 *
 * [344] 反转字符串
 */
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i = 0, j = s.size()-1;
        char temp;
        while(i<=j){
            temp = s[i];
            s[i] = s[j];
            s[j] = temp;
            i++;
            j--;
        }
    }
};
```

> - 思路：双指针简单思路

### Q58

#### (1) 题目信息

> - 标题：[反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/description/)
> - 编号&难度：[345]，easy
> - Tags：[`two-pointers`](https://leetcode.com/tag/two-pointers) | [`string`](https://leetcode.com/tag/string)
> - 描述：编写一个函数，以字符串作为输入，反转该字符串中的元音字母。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=345 lang=cpp
 *
 * [345] 反转字符串中的元音字母
 */
class Solution {
public:
    string reverseVowels(string s) {        
        int i = 0, j = s.size()-1;
        char temp;
        while(i<j){
            i = s.find_first_of("aeiouAEIOU", i);
            j = s.find_last_of("aeiouAEIOU", j);

            if (i < j){
                swap(s[i++],s[j--]);
            }
        }

        return s;
    }
};
```

> - 思路：看了solution。。因为题意一看理解错了。关键在于find_first_of、find_last_of

### Q59

#### (1) 题目信息

> - 标题：[两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/description/)
> - 编号&难度：[349]，easy
> - Tags：[`hash-table`](https://leetcode.com/tag/hash-table) | [`two-pointers`](https://leetcode.com/tag/two-pointers) | [`binary-search`](https://leetcode.com/tag/binary-search) | [`sort`](https://leetcode.com/tag/sort)
> - 描述：给定两个数组，编写一个函数来计算它们的交集。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=349 lang=cpp
 *
 * [349] 两个数组的交集
 */
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int,int> map1, map2;
        for(int i=0; i<nums1.size(); i++){
            if( map1.find(nums1[i]) == map1.end() ){
                map1[nums1[i]] = 0;
            }
        }
        for(int i=0; i<nums2.size(); i++){
            if( map2.find(nums2[i]) == map2.end() ){
                map2[nums2[i]] = 0;
            }
        }

        //begin
        vector<int> nums_inter;
        for(auto iter1 = map1.begin(); iter1 != map1.end(); iter1++){
            if(map2.find(iter1->first) != map2.end()){
                nums_inter.push_back(iter1->first);
            }
        }

        return nums_inter;
    }
};
```

> - 思路：储存2个数组的次数，遍历数组

### Q60

#### (1) 题目信息

> - 标题：[两个数组的交集 II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/description/)
> - 编号&难度：[350]，easy
> - Tags：[`hash-table`](https://leetcode.com/tag/hash-table) | [`two-pointers`](https://leetcode.com/tag/two-pointers) | [`binary-search`](https://leetcode.com/tag/binary-search) | [`sort`](https://leetcode.com/tag/sort)
> - 描述：给定两个数组，编写一个函数来计算它们的交集。**说明：**输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。我们可以不考虑输出结果的顺序。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=350 lang=cpp
 *
 * [350] 两个数组的交集 II
 */
class Solution {
public:
    void getArrayInter(vector<int> nums1, vector<int> nums2, \
        vector<int>& array_inter){
        for(int i=0; i<nums1.size(); i++){
            auto iter_find = find(nums2.begin(), nums2.end(), nums1[i]);
            if( iter_find != nums2.end() ){
                array_inter.push_back(nums1[i]);
                nums2.erase(iter_find);
            }
        }
    }

    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> array_inter;
        if(nums1.size() < nums2.size()){
            getArrayInter(nums1,nums2,array_inter);
        }
        else{
            getArrayInter(nums2,nums1,array_inter);
        }

        return array_inter;
    }
};
```

> - 思路：遍历nums1，在nums2中查找nums1数字，然后每当查找到就erase nums2中对应数字

### Q60

#### (1) 题目信息

> - 标题：[有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/description/)
> - 编号&难度：[367]，easy
> - Tags：[`math`](https://leetcode.com/tag/math) | [`binary-search`](https://leetcode.com/tag/binary-search)
> - 描述：给定一个正整数 *num*，编写一个函数，如果 *num* 是一个完全平方数，则返回 True，否则返回 False。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=367 lang=cpp
 *
 * [367] 有效的完全平方数
 */
class Solution {
public:
    bool isPerfectSquare(int num) {
        long left = 0, right = num, mid = num/2;
        while(left < right){
            if(left+1 == right){
                if(left*left == num || right*right == num){
                    return true;
                }
                else{
                    return false;
                }
            }

            mid = (left+right)/2;
            if(mid*mid < num){
                left = mid;
            }
            else if(mid*mid > num){
                right = mid;
            }
            else{
                return true;
            }
        }

        return false;
    }
};
```

> - 思路：变相二分法，注意`if(left+1 == right)`

### Q61

#### (1) 题目信息

> - 标题：[字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/description/)
> - 编号&难度：[387]，easy
> - Tags：[`hash-table`](https://leetcode.com/tag/hash-table) | [`string`](https://leetcode.com/tag/string)
> - 描述：给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=387 lang=cpp
 *
 * [387] 字符串中的第一个唯一字符
 */
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char,int> map_s;
        vector<int> vector_s;

        for(int i=0; i<s.length(); i++){
            if(map_s.find(s[i]) == map_s.end()){
                map_s[s[i]] = i;
                vector_s.push_back(i);
            }
            else{
                auto iter_find = find(vector_s.begin(), vector_s.end(),map_s[s[i]]);
                if( iter_find != vector_s.end() ){
                    vector_s.erase(iter_find);
                }
            }
        }


        return (vector_s.size())?\
            ( *(min_element(vector_s.begin(),vector_s.end())) ):(-1);
    }
};
```

> - 思路：遍历字符串，map_s储存不重复字符index，vector_s储存字符index，当出现重复字符时erase掉vector_s中对应的index。最后，返回vector_s最小值

### Q62

#### (1) 题目信息

> - 标题：[找不同](https://leetcode-cn.com/problems/find-the-difference/description/)
> - 编号&难度：[389]，easy
> - Tags：[`hash-table`](https://leetcode.com/tag/hash-table) | [`bit-manipulation`](https://leetcode.com/tag/bit-manipulation)
> - 描述：给定两个字符串 ***s*** 和 ***t***，它们只包含小写字母。字符串 **t** 由字符串 **s** 随机重排，然后在随机位置添加一个字母。请找出在 ***t*** 中被添加的字母。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=389 lang=cpp
 *
 * [389] 找不同
 */
class Solution {
public:
    char findTheDifference(string s, string t) {
        unordered_map<char,int> map;
        for(char ch_s:s){
            if(map.find(ch_s) != map.end()){
                map[ch_s]++;
            }
            else{
                map[ch_s] = 1;
            }
        }

        for(char t_ch:t){
            if(map.find(t_ch) == map.end() || map[t_ch] == 0){
                return t_ch;
            }
            else{
                map[t_ch]--;
            }
        }

        return '0';//trash code
    }
};
```

> - 思路：遍历s记录字符频数map，然后遍历t，在map中查找t字符判断`if(map.find(t_ch) == map.end() || map[t_ch] == 0)`，否则频数减一

### Q63

#### (1) 题目信息

> - 标题：[左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/description/)
> - 编号&难度：[404]，easy
> - Tags：[`tree`](https://leetcode.com/tag/tree)
> - 描述：计算给定二叉树的所有左叶子之和。

#### (2) 个人Code

```cpp
/*
 * @lc app=leetcode.cn id=404 lang=cpp
 *
 * [404] 左叶子之和
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
    void getLeftVals(TreeNode* root, vector<int>& leftNodesVals, int leftOrRight){
        if(!root->left && !root->right && leftOrRight == 1){
            leftNodesVals.push_back(root->val);
            return;
        }

        if(root->left){
            getLeftVals(root->left, leftNodesVals, 1);
        }

        if(root->right){
            getLeftVals(root->right, leftNodesVals, 2);
        }
    }

    int sumOfLeftLeaves(TreeNode* root) {
        if(!root || (!root->left && !root->right)){
            return 0;
        }

        vector<int> leftNodesVals;
        getLeftVals(root, leftNodesVals, 1);

        int sum = 0;
        for(int i=0; i<leftNodesVals.size(); i++){
            sum += leftNodesVals[i];
        }

        return sum;
    }
};
```

> - 思路：深度优先遍历 + leftOrRight Flag