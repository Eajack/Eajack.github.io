---
title: 《2018年刑侦科推理试题》Python版求解⁄(⁄ ⁄•⁄ω⁄•⁄ ⁄)⁄
date: 2018-3-7 22:16:17
tags:
  - Python
categories:
  - Python
---

昨天刷知乎想法，看到Milo Yip的[深夜暴力編程](https://www.zhihu.com/pin/954541989945905152)，感觉有点意思，所有尝试用py写了下…思路没什么，就是暴力遍历而已，纯粹一时兴起~先抛结果：BCACACDABA

Python代码如下：
```python
# !/usr/bin/env python3
# -*- coding : utf-8 -*-
# ==================================================================================================================
# Author: Eajack
# date:2018/3/7
# ==================================================================================================================
# Function：
#   2018年刑侦科推理试题，暴力遍历……
# ==================================================================================================================
# Results：
#   ['B', 'C', 'A', 'C', 'A', 'C', 'D', 'A', 'B', 'A']
#   [Finished in 5.7s]
#   经过检验，答案都是对的（如果没眼花的话……）
# ==================================================================================================================

def preparation(answers):
    global maxCount,maxOption,minCount,minOption
    numCountsDict = {i:answers.count(i) for i in set(answers)}
    numCountsList = []
    for key,value in numCountsDict.items():
        numCountsList.append([value,key])
    numCountsList = sorted(numCountsList,reverse = True)

    maxCount = numCountsList[0][0]
    maxOption = numCountsList[0][1]
    minCount = numCountsList[-1][0]
    minOption = numCountsList[-1][1]

def Q3Test(answers):
    if(answers[2] == 1):
        return (answers[5]==answers[1]==answers[3] and answers[2]!=answers[5])
    else:
        if(answers[2]==2):
            return (answers[2]==answers[1]==answers[3] and answers[5]!=answers[2])
        else:
            if(answers[2]==3):
                return (answers[2]==answers[5]==answers[3] and answers[1]!=answers[2])
            else:
                return (answers[2]==answers[5]==answers[1] and answers[3]!=answers[2])

def Q4Test(answers):
    trueList = [int(answers[0]==answers[4]),int(answers[1]==answers[6]),int(answers[0]==answers[8]),int(answers[5]==answers[9])]
    if(sum(trueList)==1 and trueList[answers[3]-1]==1):
        return True
    else:
        return False

def Q6Test(answers):
    if(answers[5] == 1):
        return (answers[1]==answers[3]==answers[7])
    else:
        if(answers[5]==2):
            return (answers[0]==answers[5]==answers[7])
        else:
            if(answers[5]==3):
                return (answers[2]==answers[9]==answers[7])
            else:
                return (answers[4]==answers[8]==answers[7])

def Q8Test(answers):
    if(answers[7] == 1):
        return (abs(answers[6]-answers[0])!=1)
    else:
        if(answers[7]==2):
            return (abs(answers[4]-answers[0])!=1)
        else:
            if(answers[7]==3):
                return (abs(answers[1]-answers[0])!=1)
            else:
                return (abs(answers[9]-answers[0])!=1)

if __name__ == '__main__':
#
    # globals
    Q2toQ5 = {1:3,2:4,3:1,4:2}
    Q4toQDouble = {1:[1,5],2:[2,7],3:[1,9],4:[6,10]}
    Q5toQX = {1:8,2:4,3:9,4:7}
    Q7toQY = {1:3,2:2,3:1,4:4}
    Q9toQZ = {1:6,2:10,3:2,4:9}
    Q10toQ = {1:3,2:2,3:4,4:1}
    answers2ADs = {1:'A',2:'B',3:'C',4:'D'}
    maxCount = []
    maxOption = []
    minCount = []
    minOption = []
#
    for Q1 in range(1,5):
        for Q2 in range(1,5):
            for Q3 in range(1,5):
                for Q4 in range(1,5):
                    for Q5 in range(1,5):
                        for Q6 in range(1,5):
                            for Q7 in range(1,5):
                                for Q8 in range(1,5):
                                    for Q9 in range(1,5):
                                        for Q10 in range(1,5):
                                            answers = [Q1,Q2,Q3,Q4,Q5,Q6,Q7,Q8,Q9,Q10]
                                            preparation(answers)
                                            # 2题 检验
                                            if(Q2toQ5[Q2] != Q5):
                                                continue;
                                            else:
                                                # 3题 检验
                                                if(Q3Test(answers) != True):
                                                    continue;
                                                else:
                                                    # 4题 检验
                                                    if(Q4Test(answers) != True):
                                                        continue;
                                                    else:
                                                        # 5题 检验
                                                        if(Q5 != answers[Q5toQX[Q5]-1]):
                                                            continue;
                                                        else:
                                                            # 6题 检验
                                                            if(Q6Test(answers) != True):
                                                                continue;
                                                            else:
                                                                # 7题 检验
                                                                if(minOption != Q7toQY[Q7]):
                                                                    continue;
                                                                else:
                                                                    # 8题 检验
                                                                    if(Q8Test(answers) != True):
                                                                        continue;
                                                                    else:
                                                                        # 9题 检验
                                                                        if((Q1==Q6) == (answers[Q9toQZ[Q9]-1] == Q5)):
                                                                            continue;
                                                                        else:
                                                                            # 10题 检验
                                                                            if(maxCount - minCount != Q10toQ[Q10]):
                                                                                continue;
                                                                            else:
                                                                                answersADs = [answers2ADs[answer] for answer in answers ]
                                                                                print(answersADs)
```