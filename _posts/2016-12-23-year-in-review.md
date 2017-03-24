---
layout:     post
title:      "2016年终总结"
subtitle:   " \"An Exciting Year\""
date:       2016-12-23 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-year-in-review.jpg"
catalog: true
tags:
    - Others
---

## 技术

2016年是动荡的一年。经历了很多神奇的事情之后，年尾终于慢慢稳定下来可以做想做的事。希望能用这样的方式来纪念这特别的一年。

在3月份左右决定实习，投了NVIDIA和深圳先进院的简历，最终去了比较自由的先进院做一些好玩的东西，同时也可以住在那边，非常方便。花了很大的力气从学校搬东西出来。

在实习的这段时间做的是和Spark相关的内容，主要是推荐系统的工程实践。由于一直以来写的都是Python Web后台开发，一点点Java和前端，对机器学习的了解局限于学校的模式识别课和一些简单的项目。接触Spark算是在自己原有的知识树上跨出了比较遥远的一步。

当初想做推荐系统的原因是，在每周助教Java课程的时候，我们助教完经常会去一些小店吃饭。久而久之，每次选择去哪家店吃什么都是一个越来越困难的选择。因此想做一个推荐系统来帮助自己决策吃什么这个问题。于是在五月份的一场Hackthon上，我们五个人用SVD算法实现了一个简单的推荐系统，然后包了一层web界面进行推荐。虽然没有拿奖，不过还是得到了很多同学的喜爱，对我们也是一个比较大的鼓励，也就决定继续做下去。

非常巧合的是，在先进院做的也是和健康有关的一些工作。在做饮食推荐的过程中也突然想到，营养水平也是用户决策过程中非常重要的一点。因此一个非常简单的想法就是，能不能结合用户的口味和身体状态，从食物的味觉和营养两个维度对用户进行推荐，这就变成了我们正在做的一个项目。一个个人的健康饮食管家。

所以，我的一个非常深刻的感受就是，不要轻视在做的任何工作，因为所做的一切都有可能在未来成为知识树上重要的一部分。他们在很大程度上决定了你的知识水平，从而影响你的决策。这成为了你与其他人区别的最主要因素。

在做这个产品的时候，由于最开始只有我一个人，因此必须后台前端通吃。同时由于时间紧迫，也强行把我逼成了一个所谓的“全栈工程师”，想一想我自己也觉得自己这一年成长了很多：学习用Spark做推荐、学习写后台、学习写移动端、学习写网页。我想回顾一下在移动端的经验，这也是从8月到12月以来的工作。

我其实有一些Android和Java的经验，因此对Android平台并不陌生。而且我一直以来很担心React Native、Ionic等所谓的Hybrid App的性能问题。在大概年初我的了解里，Hybrid还是一个很缓慢的方案。但是出于好奇，我尝试了一下React Native的设计，几乎是很快地我就适应了这种方式，而且由于它所编写的Real Mobile App，而不是Hybrid的，在iOS上的性能非常好，所以最终选择了这个方案。

React Native所编写的app具有五个特点，这五个特点每一个都比较吸引我。

1. Learn Once, Write Everywhere. 也就是Android和iOS都可以编写。不同于Java的Write Once, Run Everywhere, 针对不同平台，React Native还是需要一些细微的修改，不过语法相同。

2. Lets you build mobile apps using only JavaScript. 一直没机会学习Objective-C或者是Swift，仅用JS，太好了！

3. A React Native App is a Real Mobile App. 不是Hybrid的，性能很强。

4. Reload your app instantly. 可以即时reload，不需要重新编译，以前“编码五分钟，编译两小时”的问题可以大大简化。

5. Use Native Code When You Need To. 可以通过Bridging和Native Code相连接，在项目里我们也这样做了一些。这可以使用大量现存的Native库，也是非常重要的特性。

React Native+Nodejs的方案，几乎是最快速的可以让你成为全栈工程师的技术栈了。如果在现在，想要学习开发app，不妨尝试一下这种方式。手机百度和QQ也都开始使用React Native了，今年也算是React Native迅速发展的一年。

在八九月份的时候，我们拿到了香港数码港管理咨询公司的CCMF资助，全职完成这个项目。特别在此感谢数码港，让我们有机会可以将自己的想法付诸实践。之后深大的资助也陆续到位，青春学堂也给了很多帮助，让我们得以“苟全于乱世”。11月份开始，在这个可能不到10平米的办公室里，我们完成了很多有趣的工作。

我们为餐厅提供了一个管理界面，餐厅只需要填写菜品的成分，即可很快地计算营养评分，查看营养水平，并以二维码的形式提供给用户。这个产品也已经逐渐准备投入使用了。


## 其他

今年接触了一些电子竞技游戏(没错，就是LOL和Dota)，其实玩的也不多，加起来可能也就30个小时。相比于以前死忠的RTS动辄几天几夜玩的文明，已经是大大的缓解了。

![the Wings Gaming](http://i.amz.mshcdn.com/EBWYkvPLExkaso_mcnBvSdK7wJc=/950x534/2016%2F08%2F14%2F39%2F86e2aca96a0d4b7e82e08ab1d8ea7ec3.be27a.png)

印象比较深刻的是Wings夺冠。后来我也看了一些视频，印象比较深刻的是DC(Digital Chaos)和EG(Evil Geniuses)中[有一场](http://www.bilibili.com/video/av5806813/)。DC用战术生生将EG拖死的情节非常有趣。在人头落后的情况下，结局翻转非常精彩。

很多时候，我们都过分重视战略，希望通过战略上的勤奋来掩盖战术上的不足。或是过分重视战术，而在战略上又有很多懒惰。对于公司的创始人或是CEO来说，这两种都会成为团队的毒药。CEO必须磨砺自己在战术和战略上的双重能力。对于一个具体问题而言，要能够自己想清楚解决方式和根本逻辑，这件事的意义。不可能随便找一个人帮你把这些事情都做好，这是非常不负责任的。

但是对于普通人来说，如果没办法同时在两方面提高，我觉得应该优先提升自己的战术素养。因为在实际的工作中，我们可能更多需要的是解决一个具体的问题，或是自己生活中的问题，或是工作中遇到的问题，甚至只是一场Dota，这种时候有一个好的战术是非常有利的，而不用太多考虑战略的问题。换句话说，我认为首先应该具备解决一个具体问题的能力，否则再在战略上勤奋也只是“眼高而手低”。对于商业公司来说，这种人其实是相当忌讳的。（试想一下，有一个人天天只bb：“我觉得应该blablabal做，但是这个我做不了，找别人做”，又不具体干活。这样的人你会用吗？）

祝Wings的成员们Dota运兴隆。他们说：“这才是Dota啊”，祝保持初心。我觉得Wings的管理、付出、团队值得每一个团队（说的没错，就是你国足）参考。

> 这是一支这样的战队
不许直播
队内互喷罚款
军事化管理
按时作息
一直只有吃饭睡觉打DOTA三件事
封闭化集训
身居重庆
一心DOTA

——知乎用户Invok

## 展望

我已经非常有预见性的看到，2017年会是受苦受难的一年。其实哪一年又不是呢，适应就好。

2017年的前6个月，90%以上的精力应该都会放在项目上，希望能在2-3月的时间里有20-30家餐厅使用。然后在4-5月有100家左右，并逐渐产生盈利这样。这是我最希望看到的事情。我们这样做的原因，主要是出于证明自己的商业模式可行的目的。而非为了赚钱。

希望自己也能保持初心，在技术的道路上越走越远，也祝只谈科技在新的一年里能够成为深大五百强，实话说我觉得离南山区五百强还有距离，唉，如果在一个小县城里，我们现在早就是五百强了。