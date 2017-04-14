---
layout: post
categories: PaperReading
title: "Paper Reading: PVANET--Deep but Lightweight Neural Networks for Real-time Object Detection"
tags: [Paper Reading, Deep Learning, Object Detection]
date-string: April 05, 2017
---

## 物体检测：PVANET
最近计划重新读一遍物体检测相关的[Paper List](https://github.com/kjw0612/awesome-deep-vision#object-detection)。

第一篇文章：PVANET: Deep but Lightweight Neural Networks for Real-time Object Detection，是Intel Imaging and Camera Technology的工作。

## PVANET简介
总体来说，这篇文章并没有提出新的检测方法，本质上是在Faster RCNN的基础上提出了改进，从而在保证精度的前提下保证实时性能。结果在VOC2012上获得了第二名的精度，但速度比第一名快了40倍，i7 CPU上达到750ms，Titan X GPU上达到了46ms。

目前主流的物体检测算法流程为：CNN feature extraction + region proposal + RoI classification。包括Faster RCNN等实际上都没有超出该框架，YOLO等end-to-end方法除外。

本文同样符合该流程，重点关注的是第一阶段（特征提取）的加速改进。作者指出，region proposal计算量很小，而classification阶段已经有很多既有方法可以采用，例如：truncated SVD。所以本文重点关注的是第一阶段：CNN feature extraction。

总的设计原则是“less channels with more layers”，即让网络“又高又瘦”。主要的手段有：concatenated ReLU(C.RuLU), Inception, HyperNet, batch normalization, residual connection, learning rate schedule等。

当前精度最高的（mAP最大）模型是Faster R-CNN + ResNet-101，耗时是本文模型的40多倍。

## 具体内容
本文基本都是实现细节，复现应该方便，而且作者提供了代码：https://github.com/sanghoon/ pva-faster-rcnn

### concatenated ReLU(C.RuLU)
作者指出在比较底层的block中，节点的输出往往倾向于成对出现，即有一个x就有倾向于有一个-x这样。所以在比较底层的block中，可以使用C.RuLU来介绍一半的计算量（卷积的channels减半）。C.RuLU原理如下图：

<center>
    <img src="http://img.blog.csdn.net/20170405150032012?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="200">
</center>

### Inception
作者指出Inception对检测不同尺寸物体作用巨大。另外，考虑到速度因素，作者使用3x3卷积的叠加来代替更大的卷积和。
<center>
    <img src="http://img.blog.csdn.net/20170405150340329?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="600">
</center>

### HyperNet
Multi-scale的feature map对提升性能非常重要。和以前的方法一样，作者选择最后一层feature map一起前面的2x和4x的两个feature map。作者用2x作为参考尺寸，然后对1x使用线性差值的方法放大，而4x使用pooling的方法缩小，然后concat作为接下来的feature map。

### 网络训练
batch normalization, residual connection, learning rate schedule等不在赘述。

### Faster R-CNN
Multi-scale（HyperNet）出来的feature map的channels是512，该feature map称为convf。为了计算方面的考虑，RPN只使用了convf的前128个channels。对于R-CNN，采用了所有的512个channels。对于每个RoI，使用RoI pooling的方法得到了6x6x512的feature map，然后接FC（4096-4096-(21+84)）。COCO数据集的类别是80个，VOC数据集的类别是20类别。

### 结论
总之作者综合使用了近期深度学习方面的一些进展（concatenated ReLU(C.RuLU), Inception, HyperNet等），对Faster RCNN的特征提取部分进行了改进，从而获得了更快的性能。

作者指出这些方法和其他网络压缩和量化方法完全独立，即还可以利用这些方法继续加速。作为示例，作者采用了truncated SVD的方法对FC进行压缩，把“4096-4096”变成了“512-4096-512-4096”，并finetune。压缩后速度提升了9.6 FPS，达到了31.3 FPS。精度略有下降：mAP减低了0.9%，达到82.9%。