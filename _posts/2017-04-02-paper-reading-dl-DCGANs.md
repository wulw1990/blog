---
layout: post
categories: deep learning
title: "Paper Reading: Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks"
tags: [Paper Reading, Deep Learning, GAN]
date-string: April 02, 2017
---

## DCGANs
Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks (arxiv 1511.06434)

和[GAN开上之作: Generative Adversarial Networks（Goodfellow 2014）](http://blog.csdn.net/wulw1990/article/details/68944019)不同点：

 - 本文采用了CNN作为Generator和Discriminator的实现。
 - 本文更加强调在无监督学习方面的应用，或者更确切地，更关心在未标注的大数据集上学习可用的特征表示。在图片分类问题上展示了GAN方法可以取得和其他无监督方法相同的性能。
 - 本文提出了一些限制(a family of architecture)，用于解决GAN难以训练的问题。
 - 本文还对网络的中间结果进行可视化分析。

## 主要内容
其实本篇文章的本质贡献在于训练的一些技巧，作者给出了训练稳定DCGANs的一些guideline：

1.不要使用Pooling。Discriminator采用带stride的convolution，Generator采用带fractional-strided的convolution。作者说这样可以让网络自己去学习降采样\上采样的方法，效果比用Pooling的好。作者指出，deconvolution是错误的称呼，争取的称呼是convolution with fractional-strided。

2.BatchNormalize的使用。作者指出加BN是让CNN版本的Generator学习好的关键，但是不能所有的层都加BN，Generator的输出层和Discriminator的输入层不要加BN。

3.去掉所有的FullConnect层。作者指出去掉fc是一个趋势，比如采用Global Pooling就可以在分类问题上达到state-of-the-art的效果。Global Pooling增加了模型的稳定性，但是损失了收敛的速度。作者提出了一个折中的方法：直接把最高层特征连接到generator的输入和discriminator的输出。对于generator，输入信号（比如长度100的均匀分布采样）先经过fc层，然后reshape成一个4维的tensor，之后就可以用全卷积网络了。而对于discriminator，则是将最后一层的卷积的结果直接拉直输入到sigmoid中（取max？）。

4. 对于非线性层。作者建议：对于generator，所有的层均采用ReLU，除了最后的输出层采用Tanh；对于discriminator，所有的层均采用LeakyReLU。
<center>
    <img src="http://img.blog.csdn.net/20170401175714235?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="500">
</center>

## 实现细节
作者在三个数据集上进行了实验：Large-scale Scene Understanding (LSUN) (Yu et al., 2015), Imagenet-1k and a newly assembled Faces dataset。

一些细节：

 - 由于generator最后一层是tanh作为输出，所以需要将输入图片的像素尺度scale到[-1,1]范围内。
 - batch size: 128
 - 所有的参数初始化: norm(0, 0.02)
 - LeakyReLU: slope设置为0.2
 - 使用Adam optimizer，而不是momentum
 - learning rate使用0.0002，而不是0.001
 - momentum($\beta_1$)使用0.5，而不是0.9

## 可视化
作者强调了对隐层进行分析和可视化的重要性。

1.对隐层进行分析有主意观察有没有memorization的成分。memorization问题是训练GAN需要关心的问题，有点类似ouverfit的概念，就是generator不是真的生成了新的样本而只是记忆了见过的样本而已。如果在在隐层中发现明显的变换（sharp transitions），则可以认为是momorization的信号。

2.对隐层进行分析有助于理解generator参数空间的架构方式。

3.对隐层进行分析可以观察是否新的语义变化的引入。这个和memorization问题直接相关，显然如果generator生成的样本有新的语义变化，则说明memorization问题不大，也说明模型确实学到了好的representation。下图展示了generator并不是完全地记忆原有的图像，而是引入了新的语义变化。
<center>
    <img src="http://img.blog.csdn.net/20170402212814856?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="500">
</center>

4.对discriminator进行的feature map进行可视化，可以看到特征学习的效果。
<center>
    <img src="http://img.blog.csdn.net/20170402214029700?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>

5.对z连续性的分析，可以看到一些语义元素的渐变过程。比如下图，调节某个z的参数，可以使得画面从无窗户渐变成为有窗户。

<center>
    <img src="http://img.blog.csdn.net/20170402214212180?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600">
</center>

6.作者尝试在generator的feature map中去掉一些语义元素。设计了一个实验，去掉所有的窗户的特征。首先标注一些样本，把图中的窗户的bounding box标注出来，然后在倒数第二层的feature map之上训练了一个logitstic回归。使用回归的结果来把判断为窗户的地方的特征去掉。对比实现结果如下，可以看出虽然这种过滤方法很naive，但是确实可以看到效果，第二行确实把第一行对应的样本中的窗户用其他语义元素替代了。

<center>
    <img src="http://img.blog.csdn.net/20170402215126622?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600">
</center>


## 展望未来
作者指出，目前的GAN训练中，会出现一个问题：训练轮数多的时候，部分filter会出现振荡。以后会试图解决该问题。

另外作者还做了一些有趣的实验，比如对z进行操作。下图展示讲生成微笑女人的样本的z减掉生成不微笑女人样本的z，然后在加上不微笑男人的样本的z，得到的新的z作为generator的输入信号，就可以生成微笑的男人的样本。
<center>
    <img src="http://img.blog.csdn.net/20170402215703437?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>
