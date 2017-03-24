---
layout:     post
title:      "图像的处理——Matrix"
subtitle:   " \"Opencv源码阅读(1)\""
date:       2017-02-11 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-year-in-review.jpg"
catalog: true
tags:
    - Technology
    - Engineering
---

打算开始一个关于计算机图像处理的系列，以OpenCV的源码为引，介绍计算机中处理图像的基本技术和模式识别的一些基本方法。这个系列主要的参考资料是Opencv的Reference、Source Code和一些公开的资料，我会尽量在每一篇的结尾或必要的位置注明。

OpenCV(OPEN source Computer Vision library)是一个计算机视觉库，可以帮助我们完成对图像的基本处理，也可以用于计算机视觉、模式识别方面的应用。OpenCV基于BSD许可证发布，因此可以免费用于学术和商业用途。

### 图像

在一副图像里，最基本的是点。在OpenCV里定义了Point类来实现, Point类中的成员变量仅有x坐标和y坐标。


  