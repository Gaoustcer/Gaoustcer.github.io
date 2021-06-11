---
layout:     post
title:      Machine Learning-聚类
subtitle:   Machine Learning course
date:       2021-06-11
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - 周志华
---
# Machine Learning——聚类

## 聚类任务

### 无监督学习

* 训练样本标记未知
* 通过对无标记训练样本的学习解释数据的内在性质及规律

### 形式化描述

* 给定样本集合$D=\{x_1,x_2,\cdots ,x_m\}$
* 每个样本是n维特征向量$x_i=(x_{i1},x_{i2},\cdots,x_{in})$
* 将样本集划分为k个不相交的簇$\{C_l|l=1,2,\cdots ,k\}$，用$\lambda_j$表示样本$x_j$的簇标签，即$x_j\in D_{\lambda_j}$，聚类的结果可以用包含m个元素的簇标记向量$\lambda=(\lambda_1,\lambda_2,\cdots ,\lambda_m)$

## 性能度量

* 簇内相似度高
* 簇间相似度低

### 有参考模型的性能度量

引入参考模型，称为外部指标，用外部指标划分得到的簇为$C^*=\{C_1^*,C_2^*,\cdots,C_s^*\}$,得到的簇标记向量为$\lambda^*=(\lambda_1^*,\lambda_2^*,\cdots,\lambda_m^*)$，将样本对$(x_i,x_j)\ i<j$分为一下四类

* $a=|SS|,|SS|=\{(x_i,x_j)|\lambda_i=\lambda_j,\lambda_i^*=\lambda_j^*\}$
* $b=|SD|,|SD|=\{(x_i,x_j)|\lambda_i=\lambda_j,\lambda_i^*\neq\lambda_j^*\}$
* $c=|DS|,|DS|=\{(x_i,x_j)|\lambda_i\neq\lambda_j,\lambda_i^*=\lambda_j^*\}$
* $d=|DD|,|DD|=\{(x_i,x_j)|\lambda_i\neq\lambda_j,\lambda_i^*\neq\lambda_j^*\}$

基于外部指标得到的性能评价指标为

* `Jaccard`系数 $JC=\frac{a}{a+b+c}$
* `FM`系数 $FMI=\sqrt{\frac{a}{a+b}\times \frac{a}{a+c}}$

### 无参考模型的性能度量

考虑聚类结果的划分$C=\{C_1,C_2,\cdots,C_k\}$，定义

* $avg(C)=\frac{2}{|C|(|C|-1)}\sum_{i\leq i<j\leq |C|} dist(x_i,x_j)$
* $diam(C)=max_{i\leq i<j\leq |C|} dist(x_i,x_j)$
* $d_{min}(C_i,C_j)=min_{x_i\in C_i,x_j\in C_j} dist(x_i,x_j)$
* $d_{cen}(C_i,C_j)=dist(\mu_i,\mu_j)$

## 距离计算

## 原型聚类

### K-means

最小化平方误差，样本集$\mathbb{D}=\{x_1,x_2,\cdots ,x_m\}$被分类为$\mathbb{C}=\{C_1,C_2,\cdots ,C_k\}$

$$E=\sum_{i=1}^k\sum_{x\in C_i} ||x-\mu_i||_2^2$$

#### 算法

1. 随机选择k个样本作为初始均值向量$\{\mu_1,\mu_2,\cdots,\mu_k)\}$
2. 令$C_i=\phi$
   1. for $j=1,2,\cdots,m$ do
      1. 计算$x_j$到各个样本中心点$\mu_i$的距离$d_{ji}$
      2. 根据最近的均值向量，确定$x_j$的簇标记
      3. 将样本$x_j$划入相应的簇$\lambda_k=argmin_{i\in\{1,2,\cdots,k\}} d_{ji}$
   2. 根据样本簇的向量更新均值向量
   3. 回到2

### 学习向量量化

假设样本本身带有标签，需要使用监督信息优化算法，最终得到一组原型向量刻画聚类结构

#### 形式化

* 样本集 $D=\{(x_1,y_1),(x_2,y_2),\cdots,(x_m,y_m)\}$
* $x_j=(x_{j1},x_{j2},\cdots,x_{jn})$样本特征
* $y_i$标记类别
* 目标：学到一组n维原型向量$\{p_1,p_2,\cdots,p_q\}$，对应的类别标记$(t_1,t_2,\cdots,t_q)$，每个向量代表一个聚类簇

#### 算法

$\eta$为学习率

1. 初始化原型向量$\{p_1,p_2,\cdots,p_q\}$
2. 随机选择样本$(x_j,y_j)$
3. 计算样本$x_j$到原型向量$p_i$的距离 $d_{ji}=||x_j-p_i||_2$
4. 找出与$x_j$最近的原型向量$p_{i^*},i^*=argmin_{i\in \{1,2,\cdots,q\}}d_{ji}$
   1. 如果 $y_j=t_{i^*}$
      1. 更新圆形向量$\hat{p}=p_{i^*}+\eta\times (x_j-p_{i^*})$
   2. 否则
      1. $\hat{p}=p_{i^*}-\eta (x_j-p_{i^*})$

形式上看，如果标记类别相同，则将$p_{i^*}$向$x_j$方向靠拢，如果不同，则需要使他们远离

#### 训练结果

任意样本x，被划入与其距离最近的原型向量代表的簇中，每个原型向量定义了与之相关的一个区域，该区域中每个样本与$p_i$的距离不大于它与其它原型向量的距离$p_{\hat{i}}$

$$R_i=\{x\in \mathbb{X}|||x-p_i||_2\leq ||x-p_{\hat{i}}||_2\}$$ Voronoi剖分

### 高斯混合聚类

采用概率模型表达聚类模型，多维高斯分布如下

$$p(x)=\frac{1}{(2\pi)^{\frac{n}{2}}|\sum|^{\frac{1}{2}}}e^{-\frac{1}{2}(x-\mu)^T\sum^{-1}(x-\mu)}$$

定义高斯混合分布$p_M(x)=\sum_{i=1}^k \alpha_i p(x|\mu_i,\sum_i),\alpha_i称为混合系数，\sum_{i=1}^k\alpha_i=1$

假设样本的生成分为以下两个步骤

* 先根据$\alpha_i$选择对应的高斯混合成分
* 根据选择的混合成分进行采样

感觉有点类似logistic regression的极大似然估计，用到贝叶斯公式，转化后验分布为先验分布

$$P_M(z_j=i|x_j)=\frac{P(z_j=i)\times P_M(x_j|z_j=i)}{P_M(x_j)}=\gamma_{ji}$$其中$P_M(x_j)=\sum_{l=1}^k \alpha_l p(x_j|\mu_l,\sum_l)$

每个样本的簇标记按照其最大的后验概率确定$\lambda_j=argmax_{i\in \{1,2,\cdots,k\}}\gamma_{ji}$

求解模型参数，采取极大似然估计