---
layout: post
categories: deep learning
title: "CVPR2017: End-to-end Learning of Driving Models from Large-scale Video Datasets"
tags: [Paper Reading, Deep Learning, Autonomous Driving]
date-string: April 10, 2017
---

## 简介
采用基于感知的策略学习（perception-based policies）来完成复杂的自动驾驶行为，正在称为计算机视觉的一个重要研究方向。作者认为，虽然基于规则的方法取得了一定的成果，但是基于学习的方法才是终极方案，尤其是在处理复杂少见场景以及需要和多个agent交互作用的情况。在这个方向上已经有不少不做，例如ALVINN等。这些工作把问题定义成从图像像素到操作的映射关系，导致训练上依赖于标定的操作。而本文采用的是无需标定的数据，这样就可以利用大量众包的视频数据，基本只需要一个行车记录仪（+速度和角速度记录）。在问题定义上，把驾驶过程定义成从图像到egomotion的映射关系，而egomotion的groundtruth可以通过速度和角速度记录记录得到。作者采用了FCN-LSTM的网络框架，并把这种网络称为：Deep Generic Driving Networks。

## 原理

具体问题定义如下式：

<center>
    <img src="http://img.blog.csdn.net/20170410142523524?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="120">
</center>

其中S表示当前和历史的状态信息，包括：历史上见过的画面以及egomotion信息。A表示下一时刻应该采取的egomotion的集合，R表示对应的分数。egomotion有两种描述方式：一种是离散型，比如{直行，停车，左转，右转}；另一种是连续型：用一个二维的速度向量表示，实际训练时，对该二维向量离散成多个bin，进行mulit-nominal学习。这种问题定义和NLP中的语言模型非常相似，所以该模型也称为“驾驶模型”。

FCN-LSTM如下图：

<center>
    <img src="http://img.blog.csdn.net/20170410142733900?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="500">
</center>

其中，FCN采用的在ImageNet上pretrain过的AlexNet，去掉了pooling5，并对fc6和fc7采用dilated convolutions。作者指出FCN比直接用CNN效果要好，是因为FCN保留了更多的spatial信息。

Loss方面，作者采用了：

<center>
    <img src="http://img.blog.csdn.net/20170410143725939?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="250">
</center>

对于Accuracy，作者采用：

<center>
    <img src="http://img.blog.csdn.net/20170410143854610?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width ="120">
</center>

## 实验
模型效果：

<center>
    <img src="http://img.blog.csdn.net/20170410144007704?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>

加语义分割任务作为side-task的对比实验效果：

<center>
    <img src="http://img.blog.csdn.net/20170410144116179?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>

图中，a是不用side-task的方式，b是只用语义分割结果，c是用FCN+side-task的情况。示例中一个是远处有红灯，一个是前方车辆尾灯刹车灯亮，这两种情况中只有c较好地解决了。作者指出，对于只用语义分割的情况可能会丢失一些content信息例如灯的颜色，而加入side-task可以改进一些细微的和不常见的情况下的性能。

## 结论
主要是三个贡献：

1. 重新定义了问题，采用了图像到egomotion的映射。
2. 采用了FCN-LSTM的架构。
3. 采用语义分割作为side-task，并提高了模型性能。

