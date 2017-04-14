---
layout: post
categories: deep learning
title: "Paper Reading: ResNext--Aggregated Residual Transformations for Deep Neural Networks"
tags: [Paper Reading, Deep Learning, Network Building Block]
date-string: April 11, 2017
---

## 简介
可以认为是ResNet的升级版。原理非常简单，一张图即可搞明白：

<center>
    <img src="http://img.blog.csdn.net/20170411142838359?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="300">
</center>

这种思想可以有三种实现形式：

<center>
    <img src="http://img.blog.csdn.net/20170411143026219?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600">
</center>

作者实验发现，三种形式效果差不多，考虑计算性能，选择第三种。

作者提出一个概念：把上述building block中除了short-cut以外的支路数量称为cardinality（基数）。作者把cardinality和depth、width两个概念并列，并在实验中证明增加cardinality比增加depth或width更加有效。

<center>
    <img src="http://img.blog.csdn.net/20170411143348329?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="350">
</center>

在FLOP同样增加到2倍的情况下，增加cardinality的方法获得了最好的结果（top5:5.3%）。而且即使在FLOP只有1/2的情况下，ResNeXt也比ResNet效果好。

