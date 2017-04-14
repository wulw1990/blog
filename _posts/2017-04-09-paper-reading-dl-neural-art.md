---
layout: post
categories: deep learning
title: "Paper Reading: A Neural Algorithm of Artistic Style"
tags: [Paper Reading, Deep Learning, Style Transfer]
date-string: April 10, 2017
---

## 简介
解决的是一个有趣的问题，即给两幅图C(content)和S(style)，保留C的content，保留S的style，得到一副新的图像。例如：

<center>
    <img src="http://img.blog.csdn.net/20170408191713226?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>

CNN的不同层编码了不同抽象层次的信息，可以借助不同层的Feature Map来对生成图像的content和style进行约束，见下图：

<center>
    <img src="http://img.blog.csdn.net/20170409143704521?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="500">
</center>

## Neural Art

本文的原理非常简单。

设content图像为C，待生成的图像为X。先将X设置为随机噪声，然后作为VGG的输入，C同样也过一遍VGG，这样对于某个层l，两者都会得到feature map，分为设置为P和F，则content loss定义为：

<center>
    <img src="http://img.blog.csdn.net/20170409144031447?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="200">
</center>

固定住两个支路的所有参数，除了X本身。对X本身进行梯度下降，得到的X即是和C保持了content一致性（一定意义上）。

对于style，loss定义稍微改变一点，使用了Gram matrix。对于某个feature map F，Gram matrix为：

<center>
    <img src="http://img.blog.csdn.net/20170409144402733?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="120">
</center>

即gram matrix的维度为NxN，N为channels，每个元素的值是对第i个channel和第j个channel求内积得到的。实际上是协方差矩阵。个人理解是通过gram matrix，是的约束更多是考虑统计特征，而不是localization相关的content。

style loss定义为：

<center>
    <img src="http://img.blog.csdn.net/20170409145138541?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="160">
</center>

综合起来，loss为：

<center>
    <img src="http://img.blog.csdn.net/20170409145446393?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="280">
</center>

## 实验
一些对比实验结果如下：

<center>
    <img src="http://img.blog.csdn.net/20170409145323346?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600">
</center>

包括对不同层的选用，以及两个loss之前权重的选择等。

## 结论

使用了非常简单粗暴的方法，利用物体检测任务训练得到的模型（VGG）的特征，即可实现不错的图像风格转换的效果。其中，在style loss中对gram matrix的使用值得借鉴。

不足：这种方式需要对生成图像进行梯度下降，而VGG模型不小，耗时应该是一个主要问题。

