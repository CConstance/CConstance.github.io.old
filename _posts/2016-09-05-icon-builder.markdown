---
layout:     post
title:      "Icon Builder"
subtitle:   " \"生成不同分辨率的Logo\""
date:       2016-08-02 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-iconbuilder.jpg"
catalog: true
tags:
    - Technology
    - Engineering
---

### 缘起

之前在做app的时候，遇到了一个算不上问题的问题，安卓和iOS往往有着截然不同的分辨率要求，与此同时，一些第三方平台也有着自己图标分辨率、大小上的要求。而现有的大多数工具又往往局限于单个平台的图标生成（很多都是收费的而且）。

因此，设计的同学就提出了这个需求，希望能有一个工具可以方便地生成各种大小的图标。从批处理的角度来看是比较容易的，PIL里可以通过resize来实现，因此就有了这个需要。

### 实现

我们通过getcwd()得到当前的工作目录，之后在当前目录下遍历所有的png文件，对每个文件，按指定的分辨率来resize，在将生成的文件按新的分辨率保存到指定的文件夹（./resized）。

### 杂项

这个项目其实从代码的角度并不复杂，大约30-50行代码，加上注释就可以实现主要功能了。不过由于是出于方便设计同学使用的需求，因此最好能将其制作成命令行指令，甚至是一个图形化的程序（打算用electron实现一个）。

最终，我使用了Click来将其封装为一个命令行程序，在命令行中输入像素要求文件的文件名，和新生成logo的名字，就可以生成对应的Logo了。

其二是，我们希望能够将其上传到pip,顺便自己也体验一下正式的Python Package发布的流程。也是一件比较有意义的工作。

最终在这个很Simple的工作中，我们还特地使用了Travis CI来进行持续集成，之后在Github和PyPi上进行发布。

### Acknowledgements

在这个项目中，所使用到的开源项目有：

* [Pillow](https://github.com/python-pillow/Pillow) -The friendly PIL fork (Python Imaging Library)

* [Click](https://github.com/pallets/click) - Python composable command line utility

### Github 

* [点击查看Github项目](https://github.com/stevefermi/IconBuilder)