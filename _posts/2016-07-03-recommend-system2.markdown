---
layout:     post
title:      "动用12个Core+64G内存来找电影"
subtitle:   " \"推荐系统系列文章之二\""
date:       2016-07-03 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-recommend-sys-2.jpg"
catalog: true
tags:
    - Technology
    - Algorithm
    - Machine Learning
    - Recommend System
---

这是推荐系统的第二篇文章。

准备下载一些电影明天考完试之后看，那怎么找到我喜欢的电影呢？一个个搜索未免太浪费时间，这种时候就要出动推荐系统了。

### 背景知识

在协同过滤算法中，我们通过用户之间共同的偏好来进行推荐。其背后的假设是，如果用户A和B在某件事上有共同点，那么用户A和B在另一件事也很有可能有着近似的观点或偏好。这个概率至少是比随机选择的一个用户要高的。

![collaborative filter](https://ws1.sinaimg.cn/large/651b652ejw1f5j4kqyxjkg20du0dcww6.gif)

上图的例子展示了一个典型的推荐系统的工作流程。根据喜好相近的用户的评分状况对图中问号区域的得分进行了估计。

#### 矩阵分解

通常来说，一个用户-物品的矩阵都是非常稀疏的，原因在于用户有很多，物品有很多，但与此同时用户看过的物品、有过评分记录的物品却是非常有限的。

### 处理能力

我搭建了一个Spark的集群来进行这个操作。在这个Spark的集群里，每台机器有24个CPU和12G的内存，我没有在家用PC上运行这个程序，不过我估计也不会用太久。

### 数据来源

有了处理能力之后还要考虑到电影的数据。这里使用了Movielens的数据。

http://grouplens.org/datasets/movielens/

在我的实例中，我使用了complete的数据集，大约有150MB的大小。测试使用的话也可以使用small的数据集，会小很多。

### 数据格式

MovieLens中的数据集中，用户评分保存在了ratings.csv中，每一行的格式都是：

```
userId,movieId,rating,timestamp
```

而电影的数据则保存在movie.csv中，每一行的格式都是：

```
movieId,title,genres
```

在其中genres指的是电影的风格，其格式如下：

```
Genre1|Genre2|Genre3
```

### 推荐引擎的实现

首先我们读入这些数据：

```
complete_ratings_raw_data = sc.textFile(small_ratings_file)
complete_ratings_raw_data_header = small_ratings_raw_data.take(1)[0]
```

将其添加到一个Spark Resilient Distributed Dataset中

```
complete_ratings_data = complete_ratings_raw_data.filter(lambda line: line!=complete_ratings_raw_data_header).map(lambda line: line.split(",")).map(lambda tokens: (tokens[0],tokens[1],tokens[2])).cache()
```

