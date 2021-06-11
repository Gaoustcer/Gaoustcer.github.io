---
layout:     post
title:      Machine Learning-Decision Tree
subtitle:   Machine Learning course
date:       2021-06-11
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - 周志华
---
# Machine Learning——Decision Tree

## 基本流程

* 构造一棵用于判定/决策的树
* 包含一个根节点，若干内部节点，若干叶节点，叶子是决策结果，其它节点相当于属性测试
* 决策树的生成
  * 递归过程
  * 叶节点的属性标记为节点所含样本最多的类别

## 属性划分

基本思想：希望划分产生的样本尽可能属于统一类别

### 信息增益

样本集合D，第k类样本所占比例为$p_k$，总共有$|Y|$类，则D的信息熵定义为 $$Ent(D)=-\sum_{k=1}^{|Y|}p_k\log_2{p_k}\tag{1}$$

定义条件熵$H(Y|X)=-\sum_{j=1}^v P(X=x_j)\sum_{i=1}^kP(Y=y_i|X=x_j)\log_2{P(Y=y_i|X=x_j)}$

采用属性a进行划分，a有V个可能的取值$\{a^1,a^2,...,a^V\}$,产生V个子节点，每个子节点的权重为$\frac{|D^v|}{D}$，则按照属性a划分样本集的信息增益为

$$Gain(D,a)=Ent(D)-\sum_{v=1}^V \frac{|D^v|}{|D|}Ent(D^v)\tag{2}$$

### 增益率

避免出现过拟合，定义增益率

$$Gain\_ratio(D,a)=\frac{Gain(D,a)}{IV(a)}\tag{3}$$

并且$IV(a)=-\sum_{v=1}^V\log_2\frac{|D^v|}{|D|}\tag{4}$

## 决策树剪枝

基本思想：将子树变成叶子节点

### 预剪枝

#### 实例

1. 将根部视为一个叶子节点，得到划分精度42.9%
2. 用属性a划分，得到三个子节点$D_1,D_2,D_3$，此时测试集准确度为71.4%>42.9%，划分是合理的
3. 若对某个属性划分产生的精确度下降，则停止划分某个节点

### 后剪枝

1. 从训练集生成一颗完整决策树
2. 如果能够让测试结果编号则进行剪枝

## 连续情况和缺失值

### 连续值

二分法，信息增益表示为

$$Gain(D,a)=max_{t\in T_a} Ent(D)-\sum_{\lambda \in \{-,+\}}\frac{|D_t^{\lambda}|}{|D|}Ent(D_t^{\lambda})\tag{5}$$

### 缺失值

## 多变量决策树

将属性映射到高维空间，决策树的分类边界相当于若干个与坐标轴平行的分段