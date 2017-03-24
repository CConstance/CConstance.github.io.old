---
layout:     post
title:      "为React Native定制雷达图"
subtitle:   " \"在React Native中调用原生组件\""
date:       2016-11-05 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-react-redux.jpg"
catalog: true
tags:
    - Technology
    - Engineering
---

### 为什么

在React Native的编程中，我们难免遇到一些需要原生组件才能调起的行为，例如相机等。如果可以通过在原生组件和rn组件之间通信的方式来解决这个问题，一方面可以让react native的组件获得极大的丰富，另一方面也可以从某种程度上提高我们程序的性能。是一件有百利的事情。当然，它的编写会相对复杂。因此在实际使用时还是要慎重。

在我们最近的项目进展中，出现了要绘制一个雷达图的需求。调研之后发现在iOS上有现有的react-native库可以实现这个功能，而且功能非常完善，基本各类图表都可以完成。而在Android上却只有一个Android原生库[MPAndroid](https://github.com/PhilJay/MPAndroidChart/)和一个较基础的MPAndroid在react native上的移植[react-native-chart-android](https://github.com/hongyin163/react-native-chart-android)。这个库当时只支持barchart,candlechart,linechart等少数几种图表。没有直接对radarchart的支持，这让我们很忧伤，于是花了一个下午在原作者的基础上增加了雷达图的功能。谨以此文作为记录。

### Android和React的接口

首先来增加一个管理雷达图的类。这个类中处理和React和信息交互。简单来说，就是用```@React.props()```这样的装饰器来对Android中原生的类方法、类属性进行封装。以代码为例：
```
@ReactProp(name="webLineWidthInner",defaultFloat=0.3f)

public void setWebLineWidthInner(RadarChart chart,float webLineWidthInner){

    chart.setWebLineWidthInner(webLineWidthInner);
    
    chart.invalidate();
}
```
按照官方文档的说法，要在js中使用的属性都需要通过以setter的形式来进行暴露。进行这个工作的就是@ReactProp这一装饰器。

>Properties that are to be reflected in JavaScript needs to be exposed as setter method annotated with @ReactProp (or @ReactPropGroup). Setter method should take view to be updated (of the current view type) as a first argument and property value as a second argument. Setter should be declared as a void method and should be public. Property type sent to JS is determined automatically based on the type of value argument of the setter. The following type of values are currently supported: boolean, int, float, double, String, Boolean, Integer, ReadableArray, ReadableMap.

在这个set结束之后，我们都要调用```chart.invalidate()```方法来对图表的信息进行验证，方可使用。

### 设置传入数据（绘制图形）

最困难的一步，是每个图表都不同的部分。我们要针对雷达图的特性来处理绘制图形的方法，这也要求我们对MPAndroidChart这个库有一些了解。我们在这里的处理相对来说是比较暴力了一些，遗留了一些问题。

我们让rn部分传过来一个字典，字典中包含了数值data(array),每个部分的名字parties(array)以及图表的名字name.这些在Android中都通过一个叫做ReadableMap的类来进行传递。之后我们就开始考虑如何绘制图形的问题。按照MPAndroidChart的介绍，我们将data和parties分别存储xVals和yVals两个列表中，之后将其存入一个RadarSet中。（这里存在一个小坑是MPAndroidChart中，github上的wiki是3.0版本，而react-native-chart-Android的作者使用的是2.2.4版本。）之后只要通过chart.setData()方法来进行设置数据的操作就可以了。

这部分出现的主要问题是对MPAndroidChart库不熟，如果对它熟悉，可以对这个图表进行更深度的定制。这也是我们打算接下来的一个side project，有空可以慢慢完善它。

### 效果

![](https://ws4.sinaimg.cn/large/651b652egw1f9ipw0h2odj20fd0nngo9.jpg)