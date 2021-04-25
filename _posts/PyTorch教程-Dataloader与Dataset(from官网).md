---
layout:     post
title:      PyTorch教程-Dataloader与Dataset(from官网)
subtitle:   PyTorch
date:       2021-04-25
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - PyTorch
---
# PyTorch教程-Dataloader与Dataset(from官网)

## loading a Dataset

example: load the fashion-minist dataset form torchvision,here is parameter

* root数据存储的路径
* train标记这是训练/测试数据
* dowload=true表示如果本地无法获取就从网上下载
* `transform` `target_transform`标记feature and label transformation

## Iterating and Visualizing the Dataset

* 通过一个列表给数据集加上索引，`training_data[index]`，使用matplotlib可视化训练数据

## 自定义Dataset类

继承自torch中的dataset类，应该包含以下方法

* `__init__`方法在实例化一个类的时候被调用
* `__len__`返回数据集的长度
*  `__getitem__`给定index，从数据集加载样本，并转化为tensor，从img_label中获取信息，transform函数将读到的数据向量化并且返回向量湖大的图像，与label关联形成一个dictionary

##  Preparing your data for training with dataloader

`Dataset`类每次提取数据集的一个数据和相应的label并将其返回。我们希望一次性训练一批数据，称之为batches

`Dataloader`是一个迭代的简单API，可以使用迭代器进行操作

## 网络模型的训练

### 构建模型 直接调用lenet-5

```python
net=LeNet(class=2)
net.initialize_weights()
```

### 设置损失函数，Cross Entropy

```python
criterion=nn.CrossEntropyLoss()
```

### 设置优化器，SGD

```python
optimizer = optim.SGD(net.parameters(), lr=LR, momentum=0.9)                        # 选择优化器
scheduler = torch.optim.lr_scheduler.StepLR(optimizer, step_size=10, gamma=0.1)     # 设置学习率下降策略
```

### 迭代训练模型