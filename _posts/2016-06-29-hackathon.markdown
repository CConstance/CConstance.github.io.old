---
layout:     post
title:      "Hackathon感想"
subtitle:   " \"2016 Segmentfault Hackathon\""
date:       2016-06-29 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-hackathon-pizza.jpg"
catalog: true
tags:
    - Anything
    - Life
---

>文艺复兴时期，虽然动乱，但是掩盖不住天才的光芒。和平年代，看似平静，但是遮掩不了内心的波澜

突然想写一些文章回顾一下今年Segmentfault的Hackathon。也是我第一次参加Hackathon的活动。

从我的角度看，Hackathon可以给我们一个非常好的机会，让我们可以把其余的一切事情都抛下，专注在代码本身。通过这样一种高强度的长达24-48小时的连续的编码工作，可以让自己有一个比较明显的成就感。

因为时间比较尴尬，临近期末考试，有完整两天空闲的人并不多。最终深大有两个小伙伴和我一起来，先进院有两个研究生师兄一起来玩，最终还是凑成了这么五个人的小队。

### 分工

我们的分工比较奇葩，廖大叔主要是写前端，Jenna来写后台，两个研究生师兄来做了数据的抓取，至于我，就一直在水……得益于完善的前后端分离，效果还是不错的。主要出现的问题是一开始的时候大家可能不太熟，彼此比较羞涩，出现了一些误解或者没有商量好的事情。在这个分工的过程中更加坚定了我以后一定要做好前端、后端、数据的分离，这样尽管可能每个人的工作有稍微的增加，但合作的成本是非常小的。

### 吃

Hackathon的一个亮点就是有很多可以吃的。让自己无后顾之忧地去写代码（我大概喝了至少七瓶可乐或者雪碧或者红牛）。

总的来说这次的Hackathon活动上的饮食……excited!
![739819389877872368.jpg](https://ooo.0o0.ooo/2016/06/29/5773f8ef003fb.jpg)

还有谜一样好吃的蛋糕
![2116174879.jpg](https://ooo.0o0.ooo/2016/06/29/5773f8f01a71e.jpg)

夜宵是pizza，味道一般。
![3211604011.jpg](https://ooo.0o0.ooo/2016/06/29/5773f8efed2b2.jpg)

第二天早上的肠粉很好吃。
![655954470599680071.jpg](https://ooo.0o0.ooo/2016/06/29/5773f954f3e58.jpg)

### 项目

我们做的是一款美食推荐的产品，可以根据你的过去的饮食记录来进行一定的推荐。我主要做的也是推荐系统的调试。在比赛之前我就简单的做了一些测试，比赛开始之前还是遇到了一些问题。

主要的问题在于性能瓶颈上，当用户的饮食记录非常多的时候，我们的系统几乎不太能够在毫秒级返回正确的结果，直观的反映就是有可能出现请求时间过长，甚至超时的情况。

因为我们采用了一个奇异值分解的算法，这个算法每次会对数据库里的数据进行奇异值的分解，在数据量很大的时候他的运算需要比较长的时间。由于时间有限我们不太可能去想一个新的算法来解决这个问题，经费也不足以购买一台性能非常强悍的服务器来解决这个问题，因此最终的强行hack的实现方式就是……

不在用户每次插入数据时更新奇异值，而是每隔一段时间，大约5-10秒更新一次奇异值，这样数据端可以很迅速地返回给后台结果，而不需要经过繁冗的奇异值计算的步骤。

另外的一个spot在于对用户数据的收集上。传统的很多评分推荐系统是不考虑“跳过”这种情况的，我们对跳过的情况也进行了打分。因为对于用户来说，“跳过”这道菜也是不喜欢的一种体现。因此最终我们的评分机制有几个离散的选项：
1. 不喜欢
2. 跳过
3. 下单
4. 收藏
5. 再次下单

最终的系统会在用户首次访问时随机推荐10道左右的菜，通过这10道菜来收集你的口味，并进行相应的判断。具体的推荐系统工作方式可以看之后的另一篇文章。

### 演示

![887057592518623059.jpg](https://ooo.0o0.ooo/2016/06/29/5773fb2744133.jpg)

尽管我们在上台前一刻还在修复bug，但是还是抽了两分钟来做PPT。

![633450715400108724.jpg](https://ooo.0o0.ooo/2016/06/29/5773fbc5d1331.jpg)

演示的过程中因为没有ppt控笔，偶然发现现在的iPhone手机已经可以非常好地控制keynote了，苹果大法好。

### 感想

首先要感谢四位小伙伴一起来参加这个比赛。这是一段非常有趣的经历。而且我也觉得我们完成了一项非常有意思的项目，值得深入挖掘它的价值。
![292070101613209157.jpg](https://ooo.0o0.ooo/2016/06/29/5773fbc61b4fd.jpg)

希望将来深大也能办一些类似的活动。
![532869694.jpg](https://ooo.0o0.ooo/2016/06/29/577495ed577ec.jpg)