---
uuid: 6a819c10-e88a-11e8-b8dd-7f4713d006c9
title: 从零开始开发你第一个微信小程序教程tutorial-part1
date: 2018-11-15 11:56:22
tags:
<<<<<<< HEAD
featured_image: ./img/startpage.png
=======
featured_image: /img/startpage.png
>>>>>>> c07f0cc8fbbebeb210328fdc9baf99530a701dd1
---

如果编程只是局限于做一些按钮小组件，那么也太过单调和无聊了。在我刚开始学习编程的时候，最重要的驱动力是想写出自己的iOS App。拥有自己的App感觉是一件非常酷的事情。今天我们就以此为切入点，来逐步完成一个微信小程序。

___

* 首先要想为自己的小程序取一个喜欢的名字。
* 其次要想好这个小程序有什么功能。
* 最后我们来实现它。



我要做这样的一个小程序，它的名字叫Asuka。这个小程序的功能很简单，就是让用户可以畅所欲言。所以我需要在Asuka上面创建一个留言板，用户可以发布自己想要说的话。说干就干，我们先来做一个产品的原型设计。

产品的原型设计工作有很多，专门为iOS使用的Sketch，在线的如同墨刀等等，不过我还是偏爱adobe旗下的XD。Adobe XD从公测起我就开始使用了，并且现在是全免费的。它上手非常简单方便，而且界面设计上非常极简，对于做Flat UI design来说，是绝对完美的原型设计工具。

[Adobe XD 下载](https://www.adobe.com/hk_en/products/xd.html)

#### UI

Asuka 原型设计第一版UI如图：

![asukaui1](/img/asukaui1.png)

这是一个最精简的产品原型。从零开始做产品，一定要从少向多不断的延伸。第一版的产品只有两个功能：

1. 用户可以查看其它用户的留言
2. 用户可以创建自己的留言

因此，我们需要三个核心组件：

1. 用户留言列表
2. 用户创建留言输入框
3. 发送按钮



#### 小程序开发工具

接下来我们打开小程序开发工具来创建一个小程序Project

[小程序开发工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html?t=18111420)

大家可以按照自己的系统下载对应的小程序开发工具。

然后创建我们第一个小程序。



![startpage](/img/startpage.png)

选择紫色的Mini Program Project



![startpage1](/img/startpage1.png)

1. 在Directory选择一个你需要存放Project的目录。
2. 在Name那里创建小程序的名字，我们用Asuka。
3. 在AppId下面的两行灰色字，第二行Or use a test account mini Program/Mini Game那里，点击Mini Program，会为小程序创建一个临时的AppId
4. 点击OK后，Asuka的启动项目就创建完成了。





