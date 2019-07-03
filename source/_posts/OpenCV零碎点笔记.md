---
title: OpenCV零碎点笔记
date: 2018-02-20 22:20:17
tags:
  - C++
categories:
  - OpenCV
---

Series:**OpenCV学习笔记**

### *updating*

### 1. OpenCV常用常数
- 矩阵数据类型：`CV_<bit_depth>(S|U|F)C<number_of_channels>`

> S = 符号整型  U = 无符号整型  F = 浮点型

>E.g.:  
CV_8UC1 是指一个8位无符号整型单通道矩阵
CV_32FC2是指一个32位浮点型双通道矩阵

列出如下：

| 数 | 据 | 类 | 型 |
|--|--|--|--|
| CV_8UC1  | CV_8SC1 | CV_16U C1 | CV_16SC1 |
| CV_8UC2  | CV_8SC2 | CV_16U C2 | CV_16SC2 |
| CV_8UC3  | CV_8SC3 | CV_16U C3 | CV_16SC3 |
| CV_8UC4  | CV_8SC4 | CV_16U C4 | CV_16SC4 |
| CV_32SC1 | CV_32FC1 | CV_64FC1 | NULL |
| CV_32SC2 | CV_32FC2 | CV_64FC2 | NULL |
| CV_32SC3 | CV_32FC3 | CV_64FC3 | NULL |
| CV_32SC4 | CV_32FC4 | CV_64FC4 | NULL |

### 2.OpenCV平台配置汇总
- [【OpenCV】CodeBlocks+OpenCV3.2环境搭建](http://blog.csdn.net/wx7788250/article/details/54970903)
- [Raspberry Pi 3 + Raspbian Jessie + OpenCV 3](https://robocoderhan.github.io/2016/12/13/Raspberry%20Pi%203%20+%20Raspbian%20Jessie%20+%20OpenCV%203/)
- [How to uninstall OpenCV (Open Source Computer Vision) from Raspberry Pi - Raspbian Jessie?](http://www.srccodes.com/p/article/56/uninstall-remove-opencv-raspberry-pi-jessie-debain-make-uninstall-open-source-computer-vision-opencvlib)(需翻墙)
- [树莓派学习笔记—— 源代码方式安装opencv](http://blog.csdn.net/xukai871105/article/details/40988101) & [树莓派学习笔记——apt方式安装opencv](http://blog.csdn.net/xukai871105/article/details/41084949)
- [【OpenCV】VS2017配置OpenCV2.4.13.4（其余高版本同理）](http://blog.csdn.net/dango_miracle/article/details/78681131)