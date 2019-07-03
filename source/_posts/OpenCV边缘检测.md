---
title: OpenCV边缘检测
date: 2017-08-09 17:50:17
tags:
  - C++
categories:
  - OpenCV
---

Series:**OpenCV学习笔记**
*边缘检测*

```cpp
/*
Author:Eajack
Date:2017/8/9
Series:OpenCV笔记
Function:OpenCV边缘检测

Key Points:
    1、Canny边缘检测步骤:原图转成灰度图 => blur降噪 => Canny边缘检测 => edge作为掩码
        Canny(srcImg,edgeImg,double threshlod1,double threshold2)
    2、Sobel算子边缘提取步骤: X方向梯度 => X方向梯度 => 整体方向梯度
                                       [x,y]
        X : Sobel(srcImg,sobel_x,CV_16S,1,0,3,1,1,BORDER_DEFAULT);
        Y : Sobel(srcImg,sobel_y,CV_16S,0,1,3,1,1,BORDER_DEFAULT);
        addWeighted(abs_sobel_x,0.5,abs_sobel_y,0.5,0,sobel_dst);
*/

#include<stdio.h>
#include<iostream>
#include<vector>
#include<opencv2/opencv.hpp>
#include<opencv2/highgui/highgui.hpp>
#include<opencv2/imgproc/imgproc.hpp>

using namespace cv;
using namespace std;

int main()
{
    Mat srcImg = imread("photo.jpg");

    /*1 Canny边缘检测 */
    Mat srcClone = srcImg.clone();
    Mat dst,edge,gray;

    /*原图转成灰度图 => blur降噪 => Canny边缘检测 => edge作为掩码*/
    // 创建与src同类型和大小的矩阵(dst)
    dst.create(srcClone.size(),srcClone.type());
    // 将原图转化为灰度图
    cvtColor(srcClone,gray,CV_BGR2GRAY);
    //先用3x3 内核降噪
    blur(gray,edge,Size(3,3));
    //运行Canny算子
    Canny(edge,edge,150,50,3);
    //设置dst所有元素为0
    dst = Scalar::all(0);
    // 使用Canny输出edge作为掩码,来将原图srcClone复制到dst
    srcClone.copyTo(dst,edge);

    imshow("Canny 边缘检测",dst);
    imwrite("canny.jpg",dst);


    /*2 sobel边缘检测 */
    Mat sobel_x,sobel_y;
    Mat abs_sobel_x,abs_sobel_y,sobel_dst;

    // X方向梯度
    Sobel(srcImg,sobel_x,CV_16S,1,0,3,1,1,BORDER_DEFAULT);
    convertScaleAbs(sobel_x,abs_sobel_x);
    imshow("X方向sobel",abs_sobel_x);
    imwrite("sobel_x.jpg",abs_sobel_x);
    // Y方向梯度
    Sobel(srcImg,sobel_y,CV_16S,0,1,3,1,1,BORDER_DEFAULT);
    convertScaleAbs(sobel_y,abs_sobel_y);
    imshow("Y方向sobel",abs_sobel_y);
    imwrite("sobel_y.jpg",abs_sobel_y);
    //合并梯度（近似）
    addWeighted(abs_sobel_x,0.5,abs_sobel_y,0.5,0,sobel_dst);
    imshow("整体方向sobel",sobel_dst);
    imwrite("sobel.jpg",sobel_dst);

    waitKey(0);
}


```