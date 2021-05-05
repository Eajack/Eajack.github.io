---
title: Cpp标准库速查手册
date: 2020-2-16 20:55:44
tags:
  - C++
  - LeetCode
categories:
  - C++
  - LeetCode
---

[TOC]

# 0. 简述

个人整理Cpp标准库，整理常用的标准库函数。参考[cplusplus.com](http://www.cplusplus.com/reference/)，**主要用于LeetCode刷题等**。

# 1. C library

C++兼容C的库

## 1.1 &lt;cctype&gt; (ctype.h)

- int isalnum ( int c )（判断字符c是否：数字/小写字母/大写字母）

- int isalpha ( int c )（判断字符c是否：小写字母/大写字母）

- int isdigit ( int c )（判断字符c是否：10进制数字）

- int islower( int c )（判断字符c是否：小写字母）

- int isupper( int c )（判断字符c是否：大写字母）

- int isxdigit( int c )（判断字符c是否：16进制数字）

- int tolower( int c )（大写字母 => 小写字母）

- int toupper( int c )（小写字母 => 大写字母）

**参数：整形（包括char）；Return：0 => false；非0值 => true**

## 1.2 &lt;cmath&gt; (math.h)

数学函数，列表如下：

- double cos/sin/tan/acos/asin/atan/exp/exp2/log/log10/sqrt (double x)（数学函数：cos(x)/sin(x)/tan(x)/acos(x)/asin(x)/atan(x)/$e^{x}$/$e^{2}$/$log_{e}$/$log_{10}$/$\sqrt{x}$）

- double pow (double base , double exponent)（数学函数：$base^{exponent}=b^{e}$）

- double ceil (double x)（向上取整，e.g. 2.3 => 3；5.6 => 6）

- uble floor(double x)（向下取整，e.g. 2.3 => 2；5.6 => 5）

- int abs(int x)（绝对值函数, **int 绝对值！！！**）

- double fabs(double x)（绝对值函数, **double 绝对值！！！**）

## 1.3 &lt;climits&gt; (limits.h)

INF宏定义

- INT_MIN（$-32767 (-2^{15}+1)$ or 更小）

- INT_MAX（$32767 (2^{15}-1)$or 更大）

- UINT_MAX（$2^{32}-1$ or 更大）

## 1.4 &lt;cstddef&gt; (stddef.h)

C标准定义

- size_t（sizeof运算返回类型，属于unsigned int）

- nullptr_t（即C++的nullptr，专指空指针，为解决：C++11之前版本的语法中，NULL代表指针&0的二义性）

- NULL（即沿用C的NULL，专指数字0）

```cpp
/*
————————————————
版权声明：本文为CSDN博主「csu_zhengzy~」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_18108083/article/details/84346655
*/


#include <iostream>
using namespace std;
 
void func(void* i)
{
	cout << "func1" << endl;
}
 
void func(int i)
{
	cout << "func2" << endl;
}

//这里func重载，有2种参数，void*/int，若用C语言定义，NULL可表示2种类型：void*/int（空指针/0）
//所以，引入nullptr专指空指针，NULL专指数字0 
void main(int argc,char* argv[])
{
	func(NULL);//运行：void func(int i)
	func(nullptr);//运行：void func(void* i)
	getchar();
}
```

## 1.5 &lt;cstdlib&gt; (stdlib.h)

- double atof (const char* str)（浮点数str => 浮点数double）

- int atoi (const char * str)（整数str => 整数int）

- void srand (unsigned int seed)（配合rand使用，初始化随机数种子）

- int rand (void)（返回[0, RAND_MAX]伪随机数，RAND_MAX >= 32767）

```cpp
/* srand & rand example */
#include <stdio.h>      /* printf, NULL */
#include <stdlib.h>     /* srand, rand */
#include <time.h>       /* time */

int main ()
{
  printf ("First number: %d\n", rand()%100);
  srand (time(NULL));
  printf ("Random number: %d\n", rand()%100);
  srand (1);
  printf ("Again the first number: %d\n", rand()%100);

  return 0;
}
```

- void* malloc (size_t size)

- void* calloc (size_t num, size_t size)

- void free (void* ptr)

free & malloc 、calloc配合使用

- malloc：分配连续的内存空间（block），大小为size字节。Return：指向空间初始地址的指针。分配空间没有初始化，全部为不确定值。size == 0 => 未定义行为（标准未涉及的行为，结果未知，和具体编译器有关）。

- calloc：分配（num*size）字节的连续内存空间（block）。**和malloc唯一区别在于，会初始化为0。**size == 0 => 未定义行为（标准未涉及的行为，结果未知，和具体编译器有关）。

# 2. Containers

C++容器：

- 说明：动态数组(vector)、队列(queue)、堆栈(stack)、堆(priority_queue)、链表(list)、树(set）、关联数组(map)

- 分类

  **顺序容器**

  - array

  - **vector**

  - **deque**

  - forward_list

  - list

  **容器适配器**（基于其他容器实现）

  - **stack**：LIFO

  - **queue**：FIFO

  - **priority_queue**

  **关系型容器**

  - **set**

  - multiset

  - **map**

  - **multimap**

  **无序关系型容器**

  - unordered_set

  - unordered_multiset

  - unordered_map

  - unordered_multimap

- **通用成员函数**（大部分常用容器C）

  - C.begin()（首位iter，**用于遍历**）、C.end()（末位iter，用于遍历）。**[begin, end)**
  - C.rbegin()（末位iter，**用于逆序遍历**）、C.rend()（首位iter，用于逆序遍历）
  - C.size()（size_t，容器大小）
  - C.empty()（bool，是否为空）
  - C.clear()（void，清空自身）
  - C.swap(D)（C和D内容互换）
  - C.erase(iter)（例如：C.erase(C.begin()））
  - C.front()（首位元素值）、C.back()（末位元素值）

## 2.1 &lt;array&gt;

### (1) 特点

- 顺序储存
- 连续内存块
- 固定尺寸（因此声明时，必须连带size声明）

注意：size = 0的array有效，但是这个array不能解引用（dereferenced），也就是不能取值，否则导致未定义行为。

### (2) 常用成员函数

- 运算符[]：类似内置数组
- void fill (const value_type& val)：使得数组C所有值置为val
- void swap (array& x)：2个**size相同**的数组值对换

```cpp
/* array examples*/
#include <array>
#include <iostream>

using namespace std;

int main ()
{
    array<int,5> a = {1,2,3,4,5};
    //1- 通用成员函数
    for(auto it=a.begin(); it!=a.end(); it++){
        cout << *it << endl;
        //1 2 3 4 5
    }
    for(auto it=a.rbegin(); it!=a.rend(); it++){
        cout << *it << endl;
        //5 4 3 2 1
    }
    cout << a.size() << endl;//5
    cout << a.front() << " " << a.back() << endl;//1 5
    if(a.empty()){
        cout << "11" << endl;
    }
    else{
        cout << "22" << endl;
        //22
    }
    //2- 运算符[]
    for(int i=0; i<a.size(); i++){
        cout << a[i] << endl;
        //1 2 3 4 5
    }
    //3- fill
    a.fill(5);
    for(int i=0; i<a.size(); i++){
        cout << a[i] << endl;
        //5 5 5 5 5
    }
    //4- swap
    array<int, 5> b = {1,1,1,1,1};
    a.swap(b);
    //a => 1,1,1,1,1
    //b => 5,5,5,5,5

    return 0;
}
```

## 2.2 &lt;deque&gt;

deque，发音/dek/，双端队列（**d**ouble-**e**nded **que**ue）

**deque和vector的不同点**：deque内存不连续；vector内存连续。

### (1) 特点

- 顺序储存
- 动态数组：**可访问任意元素。增删元素：首尾快，其他慢**
- 固定尺寸（因此声明时，必须连带size声明）

### (2) 常用成员函数

- deque声明

  ```cpp
  deque<int> first;                                // 1- 空deque声明
  deque<int> second (4,100);                       // 2- 4个ints，值都为100
  deque<int> third (second.begin(),second.end());  // 3- 迭代器copy
  deque<int> fourth (third);                       // 4- 直接copy
  ```

- 运算符=

  **深拷贝！！！**

  ```cpp
  std::deque<int> first (3);    // 长度为3的0值deque
  std::deque<int> second (5);   // 长度为5的0值deque
  
  second = first;				// 赋值=，深拷贝。注意：second变成：长度为3的0值deque
  first = std::deque<int>();	// 改变first，变成空deque
  
  //空deque，first.size = 0
  std::cout << "Size of first: " << int (first.size()) << '\n';	
  //深拷贝，first没了，second还在，second.size = 3
  std::cout << "Size of second: " << int (second.size()) << '\n'; 
  ```

- 运算符[]

  **和array一样使用**

- resize函数

  - 函数声明&说明

    ```cpp
    void resize (size_type n);
    void resize (size_type n, const value_type& val);
    ```

    **设size为deque当前大小。如果n < size，则截取前n个保留；如果n > size，则尾部补充(n-size)个val值（val给定则用val值，否则置为初始值，e.g. int的初始值为0）**

- push_front()/pop_front()

  - 函数声明&说明

    ```cpp
    void push_front (const value_type& val);
    void push_front (value_type&& val);
    
    void pop_front();
    ```

    首部push & pop操作，返回值为void。会修改deque本身。

- push_back()/pop_back()

  - 函数声明&说明

    ```cpp
    void push_back (const value_type& val);
    void push_back (value_type&& val);
    
    void pop_back();
    ```

    尾部push & pop操作，返回值为void。会修改deque本身。

- insert()

  **在迭代器position前插入val值，若position为begin/end。效率不如list**


## 2.3 &lt;forward_list&gt; & &lt;list&gt;

即单向链表/双向链表。（比较少用）

`forward_list`和`list`的区别在于：前者的实现只保留前向指针，后者则是双向的。

- forward_list特点
  - 顺序储存
  - 单向链表
  - 不可以O(1)根据索引获得元素
- list特点
  - 顺序储存
  - 双向链表
  - 不可以O(1)根据索引获得元素

## 2.4 &lt;map&gt;

### (1) 特点

- key具有唯一性
- 有序的

**注意：map & unordered_map 区别。前者有序，内部红黑树实现；后者无序，内部哈希表实现。**

### (2) 常用成员函数

#### class：map

- map声明、初始化

map声明、初始化例子如下

```cpp
// constructing maps
#include <iostream>
#include <map>

bool fncomp (char lhs, char rhs) {return lhs<rhs;}

struct classcomp {
  bool operator() (const char& lhs, const char& rhs) const
  {return lhs<rhs;}
};

int main ()
{
    //1- 初始化方法1
    std::map<char,int> zero = {
        {'a',0},
        {'b',1}
    }
    
    //2- 初始化方法1
    std::map<char,int> first;//空map
    first['a']=10;
    first['b']=30;
    first['c']=50;
    first['d']=70;

    //3- 
    std::map<char,int> second (first.begin(),first.end());

    //4- 
    std::map<char,int> third (second);
	
    //5-第三参数1，根据这个参数，重新定义排序
    std::map<char,int,classcomp> fourth;                 // class as Compare

    //6-第三参数2
    bool(*fn_pt)(char,char) = fncomp;
    std::map<char,int,bool(*)(char,char)> fifth (fn_pt); // function pointer as Compare

    return 0;
}
```

- 运算符=：**深拷贝！！！**
- 运算符[]：取值，[]中是key值，返回value
- C.find(it)：map查找函数，返回值为迭代器it，当it == C.end()时表示没找到

#### class：multimap

- 和map不同之处：**key值可重复**。有用！！！

## 2.5 &lt;set&gt;

包括set & multiset，对应map & multimap。

**注意：set & multiset 区别。前者是后者的特例，前者的所有key对应的value值都是同1个值。**

## 2.6 &lt;queue&gt;

### (1) 特点

- FIFO（先进先出）
- 尾部push，头部pop

### (2) 常用成员函数

- C.push (const value_type& val)：返回值void，push进去元素
- C.pop()：返回值void，pop出头部元素

## 2.7 &lt;priority_queue&gt;

### (1) 特点

- 优先队列，也就是**堆**，默认为**最大堆**
- 默认地，用vector实现priority_queue

### (2) 常用成员函数

- priority_queue声明、初始化

```cpp
// constructing priority queues
#include <iostream>       // std::cout
#include <queue>          // std::priority_queue
#include <vector>         // std::vector
#include <functional>     // std::greater

int main ()
{
    int myints[]= {10,60,50,20};
	
    //1- 初始化1，空堆
    std::priority_queue<int> first;
    //2- 最大堆
    std::priority_queue<int> second (myints,myints+4);
    //3- 最小堆
    std::priority_queue<int, std::vector<int>, std::greater<int>>
        third (myints,myints+4);

    return 0;
}
```

- C.top()：返回堆顶元素，最大堆就是最大值，最小堆就是最小值
- C.push(val)：void返回，将val，推入到priority_queue中，**并内部重新排序，保持 最大堆/最小堆**
- C.pop()：void返回，pop出堆顶值

## 2.8 &lt;stack>

### (1) 特点

- 栈，LIFO，后进先出

### (2) 常用成员函数

- stack声明、初始化

```cpp
// constructing stacks
#include <iostream>       // std::cout
#include <stack>          // std::stack
#include <vector>         // std::vector
#include <deque>          // std::deque

int main ()
{
    std::deque<int> mydeque (3,100);          // deque with 3 elements
    std::vector<int> myvector (2,200);        // vector with 2 elements
	
    //1- 初始化1，空stack
    std::stack<int> first;                    // empty stack

    //2- 
    std::stack<int> second (mydeque);         // stack initialized to copy of deque

    //3- vector初始化stack
    std::stack<int,std::vector<int> > third;  // empty stack using vector
    std::stack<int,std::vector<int> > fourth (myvector);

    return 0;
}
```

- C.top()：返回栈顶元素
- C.push(val)：void返回，将val，推入到stack中
- C.pop()：void返回，pop出栈顶值

## 2.9 &lt;vector>

### (1) 特点

- 顺序储存
- **动态数组。维护的实际capacity > size（2倍扩增）**

### (2) 常用成员函数

- vector声明、初始化

```cpp
// constructing vectors
#include <iostream>
#include <vector>

int main ()
{
    // constructors used in the same order as described above:
    //0-
    std::vector<int> zero = {1,1,1,1};
    
    //1- 初始化1
    std::vector<int> first;                                // empty vector of ints
    
    //2- (size, value)
    std::vector<int> second (4,100);                       // four ints with value 100
    
    //3- 
    std::vector<int> third (second.begin(),second.end());  // iterating through second
    
    //4- 
    std::vector<int> fourth (third);                       // a copy of third

    return 0;
}
```

- 运算符[]：按索引获取值
- 运算符=：赋值，**深拷贝**
- C.size()：size_t返回值，返回C的大小
- C.push_back(val)：push入val值
- C.pop_back()：尾部pop，返回值void

# 3. Input/Output

## 3.1 &lt;iostream>

- cin：输入
- cout：输出

```cpp
#include <iostream>
using namespace std;

int main ()
{
    int in;
    cin >> in;
    
    cout << "in: " << in endl;
}
```

# 4. Other

## 4.1 &lt;string>

### (1) string：C++字符串

- string声明、初始化

```cpp
// string constructor
#include <string>

int main ()
{
	//1- 初始化1
    std::string s0 = "Initial string";

    //2- 空string
    std::string s1;
    
    //3- 括号
    std::string s2 (s0);
    
    return 0;
}
```

- 运算符=：**深拷贝！！！**
- 运算符+=：string后面append字符/字符串，改变自身
- C.c_str()：const char* c_str() const noexcept，获取string的C字符串形式，返回const char*指针
- C.copy(s, len, pos)：返回值为len，将C的从pos位置开始的len个字符复制给s，修改s
- C.find(s)：返回值为size_t，s的索引。若为-1，则表示没找到
- C.find_first_of(s)/C.find_last_of(s)：找到正序/逆序的第1个s的索引
- C.find_first_not_of(s)/C.find_last_not_of(s)：找到正序/逆序的第1个不是s的索引
- C.substr(pos, len)：返回C的子串，起点=pos，长度为len（默认到结尾）

### (2) 数值=>string（to_string函数）

```cpp
string to_string (int val);
string to_string (long val);
string to_string (long long val);
string to_string (unsigned val);
string to_string (unsigned long val);
string to_string (unsigned long long val);
string to_string (float val);
string to_string (double val);
string to_string (long double val);
```

**输入：整形/浮点数；输出：数字的string**

### (3) string=>数值

```cpp
stoi: string => integer
stol: string => long int
stoul: string => unsigned integer
stoll: string => long long
stoull: string => unsigned long long
stof: string => float
stod: string => double
stold: string => long double

PS: sto:i/l/ul/ll/ull/f/d/ld
```

**输入：数字的string；输出：整形/浮点数**

## 4.2 &lt;bitset>

### (1) bitset：声明、初始化

```cpp
// constructing bitsets
#include <iostream>       // std::cout
#include <string>         // std::string
#include <bitset>         // std::bitset

int main ()
{
    //初始化必须声明位数
    //1- 初始化1
    std::bitset<16> foo;//foo：0000000000000000
    
    //2-
    std::bitset<16> bar (0xfa2);//bar：0000111110100010
    
    //3-
    std::bitset<16> baz (std::string("0101111001"));//bar：0000000101111001
    
    return 0;
}
```

### (2) 常用成员函数

**注意：bitset的索引0从最后边算起，最右是0**

- 运算符[]：索引取值，类似vector
- C.count()：返回C（bitset类型）的1的数量，size_t
- C.test(pos)：返回C[pos] == 1
- C.set()：把C全部置为1。或者C.set(pos)，C[pos]=1
- C.reset()：把C全部置为0。或者C.reset(pos)，C[pos]=0
- C.filp()：把C全部0/1反转。或者C.filp(pos)，C[pos]取反
- C.to_string()：转成string
- C.to_ulong()：转成unsigned long

## 4.3 &lt;algorithm>（重点/难点）

**迭代器范围[first, last)**

以下变量符号仅供理解，不是可用符号

### （1）查找函数： Iterator find (Iterator first, Iterator last, val)：在[first, last)找到val，并返回迭代器。

### （2）统计函数： int count (Iterator first, Iterator last, val)：在[first, last)，计算val出现次数。

### （3）交换函数：void swap (a, b)：交换a/b值，深拷贝。

### （4）替换函数：replace (Iterator first, Iterator last, old_value, new_value)：old_value => new_value；原地修改。

### （5）替换函数2： Iterator replace_copy (Iterator first, Iterator last,Iterator result, old_value, new_value);：old_value => new_value；同时本身不变，结果放在result中。

### （6）最大值/最小值函数：Iterator max_element/min_element (Iterator first, Iterator last)：返回最大值/最小值对应的迭代器。

### （7）排序函数：sort (Iterator first, Iterator last)：排序[first, last)，默认升序。原地修改。

### （8）倒序函数：void reverse (Iterator first, Iterator last)：倒序[first, last)。原地修改。

### （9）倒序函数2：void reverse_copy (Iterator first, Iterator last, Iterator result)：倒序[first, last)。同时本身不变，结果放在result中。

### （10）去连续重复值函数：Iterator unique (Iterator first, Iterator  last)：去除连续重复值。原地修改。

### （11）去连续重复值函数2：Iterator unique (Iterator first, Iterator  last, Iterator  result)：去除连续重复值。原地修改。同时本身不变，结果放在result中。

```cpp
// unique：去连续重复值函数
#include <iostream>     // std::cout
#include <algorithm>    // std::unique, std::distance
#include <vector>       // std::vector

int main () {
    int myints[] = {10,20,20,20,30,30,20,20,10};           // 10 20 20 20 30 30 20 20 10
    std::vector<int> myvector (myints,myints+9);

    // using default comparison:
    std::vector<int>::iterator it;
    it = std::unique (myvector.begin(), myvector.end());   // 10 20 30 20 10 ?  ?  ?  ?
    													   //                ^

    myvector.resize( std::distance(myvector.begin(),it) ); // 10 20 30 20 10

    return 0;
}
```

### （12）全排列函数：bool next_permutation (Iterator first, Iterator last)：全排列函数

```cpp
// next_permutation：全排列函数
#include <iostream>     // std::cout
#include <algorithm>    // std::next_permutation, std::sort

int main () {
    int myints[] = {1,2,3};

    std::sort (myints,myints+3);
    std::cout << "The 3! possible permutations with 3 elements:\n";
    do {
        std::cout << myints[0] << ' ' << myints[1] << ' ' << myints[2] << '\n';
    } while ( std::next_permutation(myints,myints+3) );

    std::cout << "After loop: " << myints[0] << ' ' << myints[1] << ' ' << myints[2] << '\n';

    return 0;
    /*
        The 3! possible permutations with 3 elements:
        1 2 3
        1 3 2
        2 1 3
        2 3 1
        3 1 2
        3 2 1
        After loop: 1 2 3
  	*/  
}
```

