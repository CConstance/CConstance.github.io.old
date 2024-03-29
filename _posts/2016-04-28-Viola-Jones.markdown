---
layout:     post
title:      "人脸检测的VJ方法"
subtitle:   " \"开心相机技术原理\""
date:       2016-04-28 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-facedete.jpg"
catalog: true
tags:
    - Technology
    - Algorithm
    - Computer Vision
---
开心相机的地址：https://bbs.ifevent.com/happycam

对于这种类型的应用来说，通常都会分为几个步骤：

1. 人脸检测(Face Detection)。检测到人脸所在的区域。并进行一系列的矫正。

2. 人脸校准(Face Alignment)。人脸校准指的是在图片中寻找到鼻子、眼睛、嘴巴之类的位置。
![Face Detect & align](http://ww2.sinaimg.cn/mw690/6941baebjw1eqy1c70fhkj209o07daba.jpg)
如图中，红色的框是在进行检测，白色的点是在进行校准。

3. 信息识别(Info Recognition)。进行性别、年龄等信息的分析和识别。

这三个问题可以说每一个都是一个非常广泛的研究领域，有很多值得做的工作。本文只做一些简单的介绍。

# 人脸检测

## Viola-Jones方法
传统的人脸检测算法是Viola-Jones算法。在OpenCV中的人脸检测功能也是使用的这种算法。它有三个核心步骤：Haar-like特征、Adaboost分类器和Cascade级联分类器。

### Haar-like特征
所谓Haar-like特征其实很好理解。Haar-like特征类似于下图：

![](http://images.cnitblog.com/blog/466153/201310/23115756-851f494ea9994b6e90006949a2150c5f.jpg)

而所谓的Haar-like的特征值就是图中白色的像素值求和，求和之后与黑色的像素值做差得到的。即

```
feature=sum(white)-sum(black)
```

但是在一幅图中这样的特征是非常多的。根据Viola-Jones的论文，一幅24*24的图中这样的Haar-like特征就会达到18万种之多（具体的计算方式我们以后再说）。所以我们可以引入积分图(Integral Image)技术。积分图是一张与原图像大小完全相同的图片，不同之处在于其每一点的像素值是其左上角所有像素值的和。如图
![Integral Image](https://ooo.0o0.ooo/2016/05/20/573f0be5e3bc2.png)

在这样一幅图中，我们记某一点的像素值为ii(x)，则当我们想要计算D区域中所有像素和时，使用ii(4)-ii(2)-ii(3)+ii(1)即可。这就可以大大减少计算时间，提升效率。

### Adaboost方法

有了特征，想要得到区分函数是非常容易的。常见的SVM方法和KNN方法都是可以做到的。但尽管本身的计算并不复杂，但Haar特征还是太多了。尤其现在图片分辨率动辄成千上万。从这些特征中选取合适的特征就非常重要。从工程中得到的实践结果是，我们可以通过结合很多个弱分类器从而组合成一个强分类器。这就是Adaboost方法。用数学的方式就可以表示为：
```
F(x) =Σαf(x)
```
其中F为强分类器，f为弱分类器。x是特征向量，α为权重。Adaboost是一种序列化的方式，需要经过很多步。举个例子来看。
![Adaboost example](https://ooo.0o0.ooo/2016/05/20/573f120f7e77d.png)

在上图中，每个数据点都有一个类别的标签，不妨设红色为1，绿色为-1。每个数据点也有一个权重wt。初始权重均为1。

我们不妨先随意分一下看看。

![ada eg2](https://ooo.0o0.ooo/2016/05/20/573f120f7e77d.png)

看上去错误的还挺多的。我们可以经过几次平移选一个相对比较好的。虽然看上去就和乱分差不多。

![ada eg3](https://ooo.0o0.ooo/2016/05/20/573f1359483bb.png)

这是只有一个线性划分函数的情况下比较好的结果了，但是仍然有很多错误的结果，我们不妨把它们的权重加大。

![ada eg4](https://ooo.0o0.ooo/2016/05/20/573f13849de9c.png)

这个时候就发现了一个新的问题，类似地，我们把现在的权重情况下的数据点进行分类。

![ada eg5](https://ooo.0o0.ooo/2016/05/20/573f146bbcbac.png)

之后再加大划分错误的点的权重，并再次进行划分。

![ada eg6](https://ooo.0o0.ooo/2016/05/20/573f14bb881af.png)

反复数次后就可以在数个线性分类器（或者说弱分类器）的基础上，构造一个非线性的分类器（强分类器）。而且这个分类器还比较好地完成了分类的任务。
![ada 6](https://ooo.0o0.ooo/2016/05/20/573f14db549bc.png)

渣渣我还做了个gif展示效果。
![adaboost](https://ooo.0o0.ooo/2016/05/20/573f177219528.gif)

### Cascade分类器

在VJ方法中的第三个亮点就是使用了级联分类器。简单来说，就是先将几个通过Adaboost方法得到的强分类器进行排序，排序原则是简单的放在前边。因为通常来说人脸只占一小部分，所以可以很放心地在前几层分类器就拒绝掉大部分非人脸区域。只要前一级拒绝了，就不在进入下一级分类器，这可以大大提高速度。其本质是一颗退化决策树。

![Cascade Classifier](https://ooo.0o0.ooo/2016/05/20/573f198e7e4e3.png)

### 结果

在Viola和Jones的论文中，共建立了38层分类器来检测正面的人脸。使用了4916张人工标记的人脸，并调整到了24*24的分辨率。测试结果如下：
![results](https://ooo.0o0.ooo/2016/05/20/573f1acad82c4.png)
![results-2](https://ooo.0o0.ooo/2016/05/20/573f1b3abca96.png)