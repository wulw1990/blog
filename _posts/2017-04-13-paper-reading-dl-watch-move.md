---
layout: post
categories: deep learning
title: "CVPR2017: Learning Features by Watching Objects Move"
tags: [Paper Reading, Deep Learning, Unsupervised Learning]
date-string: April 13, 2017
---

其实做法非常简单，就是利用视频的光流信息来做前景分割，然后用模型去学习前景分割的结果。结果发现，虽有利用光流信息得到的分割结果噪声很大（pseudo ground truth），但是训练出来的模型确得到很好的特征，可以用于物体检测等。

本文最值得学习的是写作的手法，虽然方法非常简单，但是写作上比较有水平。

1.在Introduction部分，把这种利用间接非监督任务（pretext task）来学习好的特征的方法进行了归纳。作者归纳了以前的四种pretext task：Example pretext tasks include reconstruct-ing the input [3, 18, 41], predicting the pixels of the next frame in a video stream [15], metric learning on object track endpoints [43], temporally ordering shuffled frames from a video [26], and spatially ordering patches from a static im- age [6, 27]. 
而本文是一种新的pretext task。

2.展开讲自己的方法之前，先讲Evaluating Feature Representations的方法。指出了这个问题的重要性，以及以前评估方法的局限性，并提出了自己的三种更general的评估方法。有点测试先行的意思。

3.对比实验。首先直接使用ImageNet的语义分割模型（无label分割），说明无label分类的模型确实学到可可以用于其他任务的特征，而本文则是无监督的方式得到的类似特征。

其他方面没有太多好说的。使用光流得到pseudo ground truth的方法都是既有的，而且分割的噪声是更大的，但是本文证明了噪声对特征学习影响不大。

<center>
    <img src="http://7xkiab.com1.z0.glb.clouddn.com/blog/image/2017-04-13-watch-move.jpg" width="400">
</center>


