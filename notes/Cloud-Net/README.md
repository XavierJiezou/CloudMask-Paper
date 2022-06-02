# Cloud-Net: An End-To-End Cloud Detection Algorithm for Landsat 8 Imagery

## 速览

| 被引 | 下载                                          | 收录                       | 源码                                                                                            | 引用                |
| ---- | --------------------------------------------- | -------------------------- | ----------------------------------------------------------------------------------------------- | ------------------- |
| 67   | [arxiv](https://arxiv.org/pdf/1901.10077.pdf) | IGARSS 2019（EI 会议检索） | [GitHub](https://github.com/SorourMo/Cloud-Net-A-semantic-segmentation-CNN-for-cloud-detection) | BibTeX 引用格式如下 |

```BibTeX
@inproceedings{mohajerani2019cloud,
  title={Cloud-Net: An end-to-end cloud detection algorithm for Landsat 8 imagery},
  author={Mohajerani, Sorour and Saeedi, Parvaneh},
  booktitle={IGARSS 2019-2019 IEEE International Geoscience and Remote Sensing Symposium},
  pages={1029--1032},
  year={2019},
  organization={IEEE}
}
```

## 摘要

Cloud-Net 在 **Fully Convolutional Network（FCN，全卷积网络）** 的基础上做了一些改进。在基准数据集上进行实验，其 Jacard Index（ IOU） 指标优于最新方法（论文中指 FCN） 8.7%。

**关键字**：云检测， Landsat， 卫星， 图像分割

## 引言

1. 首先，介绍了云检测工作的重要性。

2. 然后，对前人的研究进行了总结，将云检测方法系统地分为三类：

- 基于阈值的方法
  - [Fmask](https://www.academia.edu/download/54703748/j.rse.2011.10.02820171012-16356-joj14a.pdf)
  - [改进的 Fmask](https://gerslab.uconn.edu/wp-content/uploads/sites/2514/2021/06/Improvement-and-expansion-of-the-Fmask-algorithm-cloud-cloud-shadow-and-snow-detection-for-Landsats-4%E2%80%937-8-and-Sentinel-2-images.pdf)
- 手工制作
  - [Haze Optimized Transformation](https://www.sciencedirect.com/science/article/abs/pii/S0034425702000342)
- 基于深度学习的方法
  - CNNs
    - [U-Net](https://link.springer.com/content/pdf/10.1007/978-3-319-24574-4_28.pdf)
    - [FCN](https://openaccess.thecvf.com/content_cvpr_2015/papers/Long_Fully_Convolutional_Networks_2015_CVPR_paper.pdf)
    - ……

3. 最后，简单描述了 Cloud-Net 能够以端到端的方式从整个遥感图像中学到局部和全局的特性，具有更好的性能。

## 方法

Cloud-Net 的结构如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/36b12147970b44df939a2a91daba19ac.png#pic_center)

<center>图1 Cloud-Net 结构</center>

相关细节：

- 数据处理
  - 使用 Landsat 8 的多光谱图像数据进行训练。其中只用了波段 2~波段 5（RGB+近红外），因为它们更通用，其它的遥感卫星也会捕捉这些波段的数据。
  - 由于单张 Landsat 8 的图像空间维度很大，所有裁成了很多个 384×384 大小的不重叠 patch。
  - 将 patch 喂给网络之前先下采样到 192×192 大小。所以实际输入到网络的尺寸是 192×192×4，网络输出的 cloud mask 尺寸是 192×192×1。
  - 数据增强使用了这些方法：水平翻转、旋转和平移。
- 模型训练
  - 最后一个卷积层之后用了 sigmoid 激活函数。（这里只做是云和不是云的判断，薄云和厚云当作云处理，不作区分，因此是像素级别是二分类问题）
  - 优化器：Adam
  - 损失函数：soft Jaccard loss function
  - 权重初始化：[-1, 1] 均匀分布
  - 学习率：0.0001
  - 学习率衰减策略：每 15 个 epoch 下调 0.7 个乘法因子，直到 10<sup>-9</sup> 后不再减小。
  - 深度学习框架：Keras

## 实验

### 数据集

> 38-Cloud: A Cloud Segmentation Dataset：点击[这里](https://www.kaggle.com/sorour/38cloud-cloud-segmentation-in-satellite-images)从 Kaggle 下载

共 38 景 Landsat 8 的图像。18 张用于训练，20 张用于测试。作者手工标注了 18 张训练图像，20 张测试图像没有手工标注，其标签是由改进的 Fmask 算法自动生成的。裁剪尺寸是 384×384，裁剪之后得到 8400 张训练图像，9201 张测试图像。

### 测试阶段

将测试集图片 resize 到 192×192，喂给训练好的模型，对网络预测的概率图进行二值化（阈值：0.047），然后 resize 到 384×384，最后拼接到原图的大小。**（这里有两点疑问：（1）二值化阈值为什么要选择 0.047？（2）图片裁剪制作 patch 采用的是不重叠的方法，对于无法整除 384 的大图该如何裁剪，裁剪后又该如何拼接成原图的大小？）**

| 1                                                                                 | 2                                                                                 |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| ![1](https://img-blog.csdnimg.cn/3e53a4fb6f6c432982ed76df574b1f44.png#pic_center) | ![2](https://img-blog.csdnimg.cn/9f9e1fe945844205b6136c8a6e21417a.png#pic_center) |

<center>图2 Cloud-Net 测试结果。a 和 e 是自然彩色图像，b 和 f 是对应的 GTs，c 和 g 是 FCN 的结果，d 和 h 是本文算法的结果</center>

### 评估指标

- Jaccard Index（IOU）
- Precision
- Recall
- Specificity
- Overall Accuracy

![在这里插入图片描述](https://img-blog.csdnimg.cn/fc158245295b4024ba5dfb611ce6f117.png#pic_center)

## 结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/4127e392ca5e4aca95f899e7eda0b127.png#pic_center)

## 总结

本文提出了一个基于深度学习的遥感图像云分割算法。与论文[[13]](https://arxiv.org/pdf/1810.05782)中的FCN 相比，具有更好的性能。并对论文[[13]](https://arxiv.org/pdf/1810.05782)的数据集进行了改进。

## 生词

- address this problem：解决这个问题
- crucial：关键的；至关重要的
- occlude：阻塞
- retrieve：检索；取回；挽回；恢复；反演
- transmission：传输
- hurricane：飓风
- volcanic eruption：火山爆发
- contract：收缩；缩小
- expand：拓展；放大
- indeed：实际上
- stitch：缝补
- superiority：优越性

*[Fmask]: 基于决策树和多阈值函数
*[改进的 Fmask]: 增加了 Cirrus 波段并被用于生成 Landsat Level-1 数据产品的 cloud mask
