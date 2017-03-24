---
layout:     post
title:      "Database Visualizer"
subtitle:   " \"一个数据可视化工具\""
date:       2016-06-29 18:00:00
author:     "Xz Yao"
header-img: "img/post-bg-data-visualizer.jpg"
catalog: true
tags:
    - Technology
    - Php
    - Visualize
---

### 数据可视化的必要性和局限

数据可视化的必要性其实无需赘述了，包括Excel里都有很多的工具。但大部分的这些工具都是需要我们手工去上传数据，然后设定图表生成。这样子有很多局限。我们更希望看到的是更多实时的图表。

举个例子，当我们需要向客户展示我们的用户增长速度的时候，一个动态增长的图表的说服力会非常强。比如说QQ的实时在线用户统计：

![qq.jpg](https://ooo.0o0.ooo/2016/06/15/57616a5ac8381.jpg)

### 一个动态生成图表并展示功能的设想

有两种方式可以比较容易地实现动态的图表。其一是让用户来完成一个标准的接口，之后可视化工具接入这个接口，取得数据，进行绘制。好处在于用户的风险较低，不存在泄露数据库密码的可能。缺点在于用户实现的成本比较高。

出于一种炫技的考虑，当然要让用户“无痛”地接入数据可视化的工具，因此我们使用了另一种方案，直接接入用户的数据库……对密码进行加密操作。

整体的流程如下：

1. 用户输入数据库的相关信息，类似于wordpress、discuz!之类的论坛一样。

2. 程序从数据库中请求相关的信息，包括所有的表名、字段名、字段数据类型。

3. 用户选择需求的表格中的元素信息。

4. 程序向数据库中请求数据，并绘制图像。

5. 将用户的分享展示出来。

### 图片展示：

添加数据库：
![login.jpg](https://ooo.0o0.ooo/2016/06/15/57616c4bb5801.jpg) 

保存的数据库信息：
![database.jpg](https://ooo.0o0.ooo/2016/06/15/57616cbc3812a.jpg)

自动取得的表名：
![tables.jpg](https://ooo.0o0.ooo/2016/06/15/57616ce7b39ab.jpg)

自动取得的字段名作为坐标轴：
![axis.jpg](https://ooo.0o0.ooo/2016/06/15/57616ce7891a5.jpg)

图表的类型：（暂时只有三种）
![types.jpg](https://ooo.0o0.ooo/2016/06/15/57616ce7e7cb6.jpg)

绘制出的图表：
![chart.jpg](https://ooo.0o0.ooo/2016/06/15/57616d3c9180d.jpg)

可被分享及评论的图表：
![shared_chart.jpg](https://ooo.0o0.ooo/2016/06/15/57616dc267a1f.jpg)

### 所使用到的工具

阿里巴巴 G2 : https://g2.alipay.com/

phpRS : https://github.com/caoym/phprs-restful

ezSQL : https://github.com/caoym/ezsql

Mustache.js : https://github.com/janl/mustache.js