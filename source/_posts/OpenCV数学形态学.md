---
title: OpenCV数学形态学
date: 2017-08-08 15:30:17
tags:
  - C++
categories:
  - OpenCV
---

Series:**OpenCV学习笔记**
*数学形态学*

```cpp
/*
Author:Eajack
Date:2017/8/8
Series:OpenCV笔记
Function:OpenCV数学形态学
KeyPoints:
	dilate(srcImg,dstImg,element);
	erode(srcImg,dstImg,element);
	morphologyEx(srcImg,dstImg,MORPH_OPEN,element);
	// MORPH_CLOSE,MORPH_GRADIENT,MORPH_TOPHAT,MORPH_BLACKHAT //
*/

# include <stdio.h>
# include <iostream>
# include <vector>
# include <opencv2/opencv.hpp>
# include <opencv2/highgui/highgui.hpp>
# include <opencv2/imgproc/imgproc.hpp>

using namespace cv;
using namespace std;

int main()
{
    Mat frame =imread("photo.jpg");
    Mat dilate_frame,erode_frame;
    Mat open_frame,close_frame,grad_frame,top_frame,black_frame;
    Mat flood_frame;
    Mat element = getStructuringElement(MORPH_RECT,Size(10,10));
    imshow("原图",frame);

    // start
    dilate(frame,dilate_frame,element);
    imshow("膨胀操作",dilate_frame);

    erode(frame,erode_frame,element);
    imshow("腐蚀操作",erode_frame);

    morphologyEx(frame,open_frame,MORPH_OPEN,element);
    imshow("开运算",open_frame);

    morphologyEx(frame,close_frame,MORPH_CLOSE,element);
    imshow("闭运算",close_frame);

    morphologyEx(frame,grad_frame,MORPH_GRADIENT,element);
    imshow("梯度运算",grad_frame);

    morphologyEx(frame,top_frame,MORPH_TOPHAT,element);
    imshow("顶帽运算",top_frame);

    morphologyEx(frame,black_frame,MORPH_BLACKHAT,element);
    imshow("黑帽运算",black_frame);

    Rect buffer;
    floodFill(frame,Point(50,300),Scalar(155,255,55),
                    &buffer,Scalar(20,20,20),Scalar(20,20,20));
    imshow("漫水填充",frame);

    imwrite("dilate.jpg",dilate_frame);
    imwrite("erode.jpg",erode_frame);
    imwrite("open.jpg",open_frame);
    imwrite("close.jpg",close_frame);
    imwrite("grad.jpg",grad_frame);
    imwrite("top.jpg",top_frame);
    imwrite("black.jpg",black_frame);
    imwrite("floodfill.jpg",frame);

    waitKey(0);
}

```