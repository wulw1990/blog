---
layout: post
categories: deep learning
title: "Paper Reading: Generative Adversarial Networks"
tags: [Paper Reading, Deep Learning, GAN]
date-string: April 01, 2017
---

## GAN开山之作
Ian J. Goodfellow的工作，GAN开山之作。GAN的基本思路：同时训练两个模型G和D，G的任务是尽量生成让D无法区分的样本，D的任务是尽量区分真实样本，总之就是让G和D在不断相互对抗(GAN)中共同提高，最终的结果是得到一个好的生成型模型。在GAN之前，深度学习在区分型模型方面取得了很大的成功，而在生成型方面几乎没有什么强项。

> GAN蕴含了阴阳相生相克的思想，和[活体检测攻防战](http://news.ifeng.com/a/20170317/50792264_0.shtml)的“魔高一尺道高一丈”的思想不谋而合。两者都是“警察抓小偷的故事”，“小偷”要尽己所能掌握可以欺骗“警察”的技术，而“警察”则要尽己所能去识别“小偷”的伎俩，在这个过程中，双方的能力都在不断地提高。

GAN作为一种思想，其使用方式可以覆盖各种模型和算法，可以说是博大精深。让人不禁想到金庸的《射雕英雄传》里面的左右互搏术。左右互搏术是金庸先生小说中周伯通被黄药师困在桃花岛十五年，百无聊赖中，别出心裁，创出来的。初期想法在于左手与右手打架，以自愉自乐，后来被郭靖点醒，明白此门武学之厉害，遂无敌于天下。

> 常言道：“心无二用。”又道：“左手画方，右手画圆，则不能成规矩。”这双手互搏之术却正是要人心为二用，一神守内，一神游外，双手使不同武功招数。临敌之时，将这套功夫使出来，分进合击，那便等于以二对一。（见金庸《射雕英雄传》）

另外，这种两条腿走路的方式，也类似EM算法，以及SML/PCD等。

## 本文方法
本文中，给出的是GAN的一种特殊情况，G采用的以随机噪声为输入的多层神经网络，D也是一个多层神经网络。优雅的是，Goodfellow还提供了论文的源代码（https://github.com/goodfeli/adversarial），不愧是good fellow。

GAN分成G和D两个部分。G部分可以表示为$G(\boldsymbol{z};\theta_g)$，其中$\boldsymbol{z}$为随机噪声，服从的分布为$p_z(\boldsymbol{z})$。D部分可以表示为$D(\boldsymbol{x};\theta_d)$，其输出结果是一个标量。设G生成的样本符合分布$p_g$，则$D(\boldsymbol{x})$表示输入$\boldsymbol{x}$来自真实样本（而非$p_g$）的概率。优化的目标：

<center>
    <img src="http://img.blog.csdn.net/20170401115027469?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>

该目标函数描述了一个minimax game（“警察抓小偷”的故事、“左右搏击术”以及“阴阳相生相克”的思想）。

到此，这篇文章基本就介绍差不多了。。。后面是一些理论分析和实现细节。
## 实现细节

1.每次更新G后，都把D训练好，在计算上是不显示的。所以作者采用了k+1的方式，每一轮迭代中，D训练k步，G训练1步。这样如果G的更新速度足够慢，D就一定会收敛到最好的分类性能。

2.刚开始时由于G生成的数据会很“假”，所以D会很容易分出所有的样本，这样导致接下来训练G的loss是0，从而无法继续学习。解决方法是修改loss function，从$\log(1-D(G(\boldsymbol{z})))$改为$\log(D(G(\boldsymbol{z})))$。

3.下图是GAN的训练过程展示。其中z表示输入的随机噪声，z上面的箭头表示G的生成过程，生成的样本满足绿色实线的分布。黑色虚线表示实际样本的分布情况。蓝色虚线表示G的参数更新过程，使得最后G产生的样本分布和实际样本的分布一致。

<center>
    <img src="http://img.blog.csdn.net/20170401134009392?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>

理论证明部分略过。。。最后作者给出了一些可能将来做的点。
## 未来展望

 - A conditional generative model $p(x|c)$ 
 - Learned approximate inference: predict z given x.
 - $p(x_S|x̸_S)$, a family of conditional models that share parameters
 - Semi-supervised learning
 - Efficiency improvements

## 效果展示

生成效果展示：a) MNIST b) TFD c) CIFAR-10 (fully connected model) d) CIFAR-10 (convolutional discriminator and “deconvolutional” generator)

<center>
    <img src="http://img.blog.csdn.net/20170401140344153?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VsdzE5OTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" width="400">
</center>