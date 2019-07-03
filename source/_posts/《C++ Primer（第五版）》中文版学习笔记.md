---
title: 《C++ Primer（第五版）》中文版学习笔记（1）
date: 2017-10-10 22:52:17
tags:
  - C++
categories:
  - C++
---

** Series ：《C++ Primer（第五版）》中文版学习笔记 **

### 第1章 开始
* C++ 标准输入输出流库：`iostream`
	* cin：标准输入
	* cout：标准输出
	* cerr：输出警告&错误消息
	* clog：输出程序运行时一般信息

```cpp
// 示例代码：
#include <iostream>

int main()
{
	std::cout << "Enter 2 numbers:" << std::endl;
	int v1 = 0, v2 = 0;
	std::cin >> v1 >> v2;
	std::cout << "The sum of " << v1 << "and" << v2
              << " is " << v1 + v2 << std::endl;

    return 0;
}

	
```