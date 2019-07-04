---
layout: post
categories: PaperReading
title: "CVPR2017: TGIF-QA: Toward Spatio-Temporal Reasoning in Visual Question Answering"
tags: [Paper Reading, Deep Learning, VQA]
date-string: April 17, 2017
---


## 简介
哈哈哈
前两天和一个产品经理的朋友聊天，再次认识到不同领域的人的交流的重要性。尤其对于当前的人工智能浪潮下，科技公司的研究部分非常接近学术前沿，甚至某种意义上超越学术界的前沿。在这样的公司要做好产品经理，最好对这些前沿技术有一个宏观的把握。对于大部分的产品经理，由于社会分工和学习的成本的因素，不可能和研究组一样去学习所有的细节。但是专业领域的人去表达的时候，同样由于交流成本的因素，常常会假定受众已经有一定的专业基础。或者另一个极端是，对于现在充斥朋友圈的所谓的科普文章，其实很多并没有讲清楚基本的概念。这些因素导致了，外行的人去看，常常是雨里雾里，增加了学习的成本。这就体现出懂技术的人能够深入浅出讲清楚专业知识的重要性。这方面的工作非常可贵，例如吴军的《数学之美》等工作。基于这个出发点，以后我在写东西的时候，尽量对受众的专业基础不做太多假设，尽量保证中学生水平也可以有所收获。

本文做的问题是VQA问题，即visual question answering。VQA基本可以理解为：给一幅图像或者视频，给一个相关问题，算法需要给出该问题的答案。一幅图说明如下：

<center>
    <img src="http://7xkiab.com1.z0.glb.clouddn.com/blog/image/VQA.jpg" width="300">
</center>

之前有很多VQA方面的工作，本文的主要工作有三个方面：

1. 做视频级别的VQA，同时考虑时域和空域两方面的因素。而以前大部分的VQA都是做图片级的问题。
2. 构造了一个数据集：TGIF-QA，一个大量的视频级VQA的数据集。
3. 方法上使用LTSM：具体地，集合了时域和空域Attention的双边LSTM。简单理解，LSTM是一个带记忆的遗忘功能的学习单元，常常用于序列学习；Attention机制是寻找时域和空域上该重点关注的区域。

文中给了代码链接，但是目前里面还没有代码。http://vision.snu.ac.kr/projects/tgif-qa

## TGIF-QA Dataset
TGIF-QA是基于TGIF的拓展。TGIF数据集顾名思义里面的视频都是按照GIF方式存储的。
57K个GIF，每个给两对QA，共计约104K对QA。

除了包括图片级的QA，该数据集引入了三种视频级特有的QA，所以共计有4中类型的QA：

 - 某个动作的重复的次数，比如：图片中的人点头了几次？
 - 重复多次的是哪个动作？
 - 状态变换过程，比如：图片中的人点完头干了什么？
 - 图片级的QA，比如：图片中的人是不是在跳舞？这种其实往往从单图就可以做。

<center>
    <img src="http://7xkiab.com1.z0.glb.clouddn.com/blog/image/video-VQA.jpg" width="450">
</center>

## 方法
方法上，其实就是在dual-LSTM基础上结合了spatial-attention[^1]和temporal-attention[^2]。一幅图说明：

[^1]: <https://arxiv.org/abs/1409.0473>
[^2]: <https://arxiv.org/abs/1502.03044>

<center>
    <img src="http://7xkiab.com1.z0.glb.clouddn.com/blog/image/video-VQA-network.jpg" width="500">
</center>

其中，Attention部分具体为：

<center>
    <img src="http://7xkiab.com1.z0.glb.clouddn.com/blog/image/video-VQA-network-attention.jpg" width="300">
</center>

## 结果
<center>
    <img src="http://7xkiab.com1.z0.glb.clouddn.com/blog/image/video-VQA-result.jpg" width="600">
</center>