---
title: ParseNet: Looking wider to see better翻译
date: 2017-03-04 12:51:00
categories:
- Deep_Learning
- Paper
tags:
- ParseNet
description: 《ParseNet: Looking wider to see better》 ICLR 2016 submission
---

### ParseNet: Looking wider to see better翻译

#### Abstract
我们展示了一种技术，添加global context到全卷积神经网络，用于semantic sengmentation.方法很简单，使用每一层的average feature增强每个位置的features.另外，我们研究了多个训练的idiosyncrasy，显著提高了baseline networks的性能.当我们添加了我们提出的global feature，和一个学习正则化参数的技术，精度不断提高，甚至超过了改进版本的baseline.我们的方法，在SiftFlow和PASCAL-Context上相对于baseline增加少量的运算代价可以达到最先进性能，通过一个简单的方法，在PASCAL VOC 2012 semantic segmentation上可以解决最先进性能.参考代码：https://github.com/weiliu89/caffe/tree/fcn.

#### Introduction
semantic segmentation，在过去十年中被广泛研究，它融合了image segmentation和object recogniton去生成图像内容中每个像素的标签.当今，用于semantic segmentation的最成功的技术是基于FCN的.FCN是从用于对整张图片进行分类的网络中提取出来的，已经被证明了优异的性能.FCN可以认为在输入图片上左右滑动一个的分类网络，并独立处理每一个滑动窗口区域.特别的是，FCN不理会图片的global information，因此忽略了潜在的有用的scene-level semantic context.为了集成更多的context，多个方法提出用graphical models中的技术，例如CRF，从而引入global context和结构化信息到FCN，尽管功能很强大，这些结构可能是复杂的，有微调一个深度神经网络和一个CRF的挑战，并且需要在管理训练方法和参数特性上有大量的经验.至少，这会导致耗时的训练和接口.
<br/>
在这个文章中，我们提出了ParseNet，一种end-to-end的简单的并且有效的卷积神经网络(CNN)，用于semantic segmentation.我们的最主要的贡献之一是，用global context来帮助使模糊变清晰.从之前的工作可以看出，为semantci segmentation添加global context并不是一个新的idea，并且到目前为止已经被应用到patch-based框架中.patch-based方法已经被大量采用到检测和分割工作，甚至集成global context到图片分类区域或物体也取得了收益.我们的方法允许集成global context到一个end-to-end的用于semantic segmentationFCN（区别于patch-based方法），添加少量的计算量.在我们的设置中，图片不是被分割成区域或物体，而是网络会做一个关于所有像素值的联合预测.以前的关于FCN的工作不包括global features，并且在保证标签的保持某些一致性的时候会对像素距离存在限制.
<br/>
添加global context到FCN很简单，但是这除了能提高FCN的精确度外，还有多个重要的结果.首先，这个完整的end-to-end过程是一个深度网络，它确保与深度网络加CRF比起来更加直截了当.另外，我们添加global context这种方法引入的计算量相对与训练和评估一个标准的FCN来说是很小的，然而性能提升很显著.在我们的方法中，对于一层的feature map被在整个图片池化，并生成一个context vector.这被追加到每个被发送到网络后续层的feature上.在实施上，这会通过unpooling这个context vector并添加resulting feature map到标准的feature map.这个技术可以被选择性得应用到一个网络的feature maps，也可以应用于结合多个feature map和信息，根据自己的需求而定.　注意来自不同层的特征可能是非常不同的，这导致直接结合feature map用于测试十分困难.我们找到了用于每一层的L2 正则化feature，并且使用一个从bp过程中学习到的一个scaling factor(缩放因子)，这两个技巧在解决潜在困难上表现很好.
<br/>
在第4部分，我们验证了这种方法，添加从一个feature map中池化得到的global context并进行一个合适的缩放，它对于basic FCN有显著的性能提高，得到了和《Semantic image segmentation with deep convolutional nets and fully connected crfs》方法中同样的精度，后者在后处理中使用了详细的结构信息.即便如此，我们也不提倡忽略结构信息.相反，我们静定添加global feature是一种用于通过考虑contextual information来提高FCN表现的简单并鲁棒的方法.事实上，我们的网络可以与精确的结构输出预测，例如CRF，这可能进一步提高性能.
<br/>
文章的剩余部分如下组织，Section 2 我们回顾了相关工作.在Section 3，我们对提出的方法进行了描述，Section　4是我们进行了广泛的实验验证.Section 5我们总结了工作，并描述了未来的工作方向.

### Conclusion
在这个工作中，我们展示了ParseNet，一个简单的FCN架构，它允许直接把global context用于semantic segmentation课题.我们明确表明了，依托FCN网最大的感受野不提供足够的global context，最大的经验感受野是不足以捕捉global context–建模global context被需要.在PASCAL VOC2012测试机上，ParseNet的分割结果是在DeepLab-LargeFOV-CRF的标准偏差中，这暗示添加global feature和带有图模型的FCN后处理预测有相似的效果.开发和分析这个方法的时候，我们提出了许多结构选择分析，讨论了最好的训练实践，说明了正则化和在网络中多层组合特征时学习权重的重要性.我们的训练显著提高了我们用的baseline的性能.我们在三个benchmark数据集展示的结果，是目前在SiftFlow和PASCAL-Context的最好的水平，在PASCAL VOC2012上接近最好的水平.最低朴素的和简单的训练，我们发现结果很令人鼓舞.在我们未来的工作，我们正探究把我们的网络与结构化的训练/接口结合.
