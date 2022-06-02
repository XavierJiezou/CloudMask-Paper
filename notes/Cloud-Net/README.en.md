# Cloud-Net: An End-To-End Cloud Detection Algorithm for Landsat 8 Imagery

## Abstract

This algorithm consists of a **Fully Convolutional Network (FCN)** that is trained by multiple pathces of Landsat8 images. Our experimental results prove that the proposed method outperforms the state-of-the-art method over a benchmark dataset by **8.7% in Jacard Index**.

**Index Terms**：Cloud detection, Landsat, satellite, image segmentation

## Introduction

Firstly, It explains the importance of cloud detection research.

And then cloud detection methods proposed by predecessors are summarized and divided into three categories:

- Threshold-based
  - [Fmask](https://www.academia.edu/download/54703748/j.rse.2011.10.02820171012-16356-joj14a.pdf)
  - [Improved Fmask](https://gerslab.uconn.edu/wp-content/uploads/sites/2514/2021/06/Improvement-and-expansion-of-the-Fmask-algorithm-cloud-cloud-shadow-and-snow-detection-for-Landsats-4%E2%80%937-8-and-Sentinel-2-images.pdf)
- Handcrafted
  - [Haze Optimized Transformation](https://www.sciencedirect.com/science/article/abs/pii/S0034425702000342)
- Deep learning-based
  - CNNs
    - U-Net
    - FCN

Lastly, Cloud-Net is capable of learning both local and global features from the entire scene in an end-to-end manner.

## Method



## New Words and Expressions

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

*[Fmask]: consists of a decision tree and is based on multiple threshold functions.
*[Improved Fmask]: benefit from Cirrus band and is utilized to produce cloud mask of the Landsat Level-1 data products.
