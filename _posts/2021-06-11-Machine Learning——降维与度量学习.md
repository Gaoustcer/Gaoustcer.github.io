---
layout:     post
title:      Machine Learning——降维与度量学习
subtitle:   Machine Learning course
date:       2021-06-11
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - 周志华
---
# Machine Learning——降维与度量学习

## k-近邻学习(KNN)

### 基本思想

给定测试样本，基于某种距离度量找出训练集中与其最邻近的k个训练样本(分类)

### 问题

#### 距离的衡量

* 欧式距离$D(x,\hat{x})=\sqrt{(x-\hat{x})^T(x-\hat{x})}$
* 标准化(避免单位不同造成的影响),将变量$x_i$变为$\frac{x_i-\mu_i}{\sigma_i}$

$$D(x,\hat{x})=\sqrt{(x-\hat{x})^T\sum ^{-1}(x-\hat{x})}\ \sum=I(i==j)\sigma_i^2$$

#### k的取值？

Validation Set

### 问题简化——最近邻分类问题

给定测试样本**x**,最近邻样本为**z**,最近邻分类器出错的概率为

$$Pr(err)=1-\sum_{c\in Y}Pr(c|x)Pr(c|x)$$

如果我们使用贝叶斯分类器，那么应该最大化$Pr(c|x)$，即$c^*=argmax_{c\in Y}Pr(c|x)$，在密采样的前提下，即测试样本**x**任意小的$\delta$附近距离范围内总能找到一个训练样本，最近邻分类器的泛化错误率不超过贝叶斯最优分类器的两倍

## 低维嵌入

高维情况下样本稀疏，不满足密采样假设，同时计算距离困难，称为维度灾难

### 降维的概念

将高维属性空间转化为一个低维子空间，在子空间中样本密度提高，距离计算更加容易。特征存在于高维的少数维度当中，即高维空间的嵌入。

### MDS

保持原始空间的距离在低维空间中的距离，引入“多维缩放(MDS)”

#### 描述

m个样本在原始空间的距离矩阵为$D\in R^{m\times m},d_{ij}$为样本$x_i,x_j$之间的距离，我们获取样本在${d}'$空间中的表示$z_i,z_j$，要求任意两个样本在${d}'$空间中的距离等于原始空间中的距离$$||z_i-z_j||=d_{ij}$$ 

$B=Z^TZ\in R^{m\times m}$为降维后的内积矩阵，$b_{ij}=z_i^Tz_j,||z_i-z_j||^2=b_{ii}+b_{jj}-2b_{ij}$

假设降维后的样本**Z**被中心化，即$\sum_{i=1}^m z_i=0$，则矩阵B的行列之和均为0，即$\sum_{i=1}^mb_{ij}=\sum_{j=1}^m b_{ij}=0$，于是我们有

$$\sum_{i=1}^m dist_{ij}^2=tr(B)+mb_{jj}\\ \sum_{i=1}^m dist_{ij}^2=tr(B)+mb_{ii}$$