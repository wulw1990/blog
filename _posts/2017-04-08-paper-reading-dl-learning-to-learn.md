---
layout: post
categories: deep learning
title: "CVPR2017: Learning to learn by gradient descent by gradient descent"
tags: [Paper Reading, Deep Learning, Learning to Learn]
date-string: April 08, 2017
---

## 简介
昨天DeepMind开源了基于TensorFlow的深度学习库——[Sonnet](https://deepmind.com/blog/open-sourcing-sonnet/)。DeepMind之前发布的learning to learn的[代码](https://github.com/deepmind/learning-to-learn)实际上就是基于Sonnet实现的。

本文解决的是优化算法的学习问题。具体来说，机器学习中我们经常可以把优化目标表示成：

<center>
    <img src="http://img.blog.csdn.net/20170408184112158?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="150">
</center>

标准的优化过程是更新如下过程：

<center>
    <img src="http://img.blog.csdn.net/20170408184217208?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="180">
</center>

这样的做法只考虑了一阶导数而不考虑二阶导数，显然会损失性能。典型的方法是通过Hessian matrix等方法来弥补。而现代的更多的方法则是针对具体类问题，比如深度学习领域经常使用的方法有：momentum、Rprop、Adagrad、RMSprop、ADAM。使用这些方法的一个最大问题是：在不同的问题上需要选择不同的优化方法。

本文试图寻找一个大一统的方案：采用RNN来学习参数更新过程，而不是手动去选择不同的优化方法。如下式：

<center>
    <img src="http://img.blog.csdn.net/20170408184811304?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="180">
</center>

这样问题变成如何去学习函数g。作者把该问题称为：Learning to learn。

## Learning to learn
作者采用RNN来做Learning to learn，即学习每次迭代的参数更新。显然，采用RNN这种带记忆功能的网络来学习更符合这里的需求（克服传统方法只考虑一阶导数的缺点）。理想的优化目标为：

<center>
    <img src="http://img.blog.csdn.net/20170408185028530?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="160">
</center>

即希望这样的参数更新更有利于f的下降。

对于RNN，假设选择记忆的长度为T，则优化目标为：

<center>
    <img src="http://img.blog.csdn.net/20170408185413135?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>


其中m表示RNN，h表示其隐状态。本文的实验中，所有的权重w均取1。

考虑到如果同时学习所有参数，会导致RNN的参数量过大，所以采用了Coordinatewise的方法。可以认为是把参数划分成不同的等分，然后共享参数，但是不共享隐状态。如下图：

<center>
    <img src="http://img.blog.csdn.net/20170408185747949?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="300">
</center>

## 实验
作者做了四个实验：

 - Quadratic functions
 - Training a small neural network on MNIST
 - Training a convolutional network on CIFAR-10
 - Neural Art

均证明了该方法比其他优化方法更好：收敛快且loss更低，泛华性能也很好。

例如，Neural Art实验，loss变化曲线：

<center>
    <img src="http://img.blog.csdn.net/20170408190041014?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>

效果：

<center>
    <img src="http://img.blog.csdn.net/20170408190120124?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600">
</center>

## 结论
用Learning to learn的方法来作为优化算法，比其他手工选择的优化算法都要好！
