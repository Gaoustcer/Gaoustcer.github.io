---
layout:     post
title:      PyTorch教程——Dataloader && DataSet
subtitle:   PyTorch
date:       2021-04-25
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - PyTorch
---
# PyTorch教程——Dataloader && DataSet

## 数据模块

* 数据收集，数据+标签
* 数据划分
* 数据读取，Dataloader,包括Sampler&&DataSet,Sampler功能为生成索引，Dataset根据生成的索引读取样本和标签
* 数据预处理 transforms

## Dataloader && Dataset

### torch.utils.data.DataLoader

```python
torch.utils.data.DataLoader(dataset, batch_size=1, shuffle=False, sampler=None, batch_sampler=None, num_workers=0, collate_fn=None, pin_memory=False, drop_last=False, timeout=0, worker_init_fn=None, multiprocessing_context=None)
```

 构造可迭代的数据装载器

* dataset: Dataset类，决定数据从哪里读取以及如何读取
* Batchsize一次读入一批数据，指定批大小
* num_works多进程读取数据
* sheuffle每个epoch是否乱序
* drop_last样本数不能被batchsize整除时，是否舍弃最后一批数据

#### 几个概念

* epoch 所有训练样本都已经输入到模型中，称为一个epoch
* iteration 一批样本被输入到模型中
* batchsize决定一个iteration有多少样本，决定一个epoch有多少iteration

### Torch.utils.data.Dataset

是一个抽象类，所有自定义的Dataset都需要继承自该类，并且重写`__getitem()__`和`__len()__`方法，前者是接受一个索引，返回索引对应的样本和标签，后者是返回所有样本的数量

数据读取需要解决的问题包括

* 读取哪些数据 每个iteration读取哪些数据
* 从哪里读取数据 文件路径
* 如何读取数据

自定义类定义如下

```python
class CustomDataset(data.Dataset):#需要继承data.Dataset
    def __init__(self):
        # TODO
        # 1. Initialize file path or list of file names.
        pass
    def __getitem__(self, index):
        # TODO
        # 1. Read one data from file (e.g. using numpy.fromfile, PIL.Image.open).
        # 2. Preprocess the data (e.g. torchvision.Transform).
        # 3. Return a data pair (e.g. image and label).
        #这里需要注意的是，第一步：read one data，是一个data
        pass
    def __len__(self):
        # You should change 0 to the total size of your dataset.
        return 0
```

`getitem函数`接受一个index，返回数据和标签，index指的是一个list的index，这个list里的元素包括了图片数据的路径和标签信息，制作这个list的方法是讲路径写在一个txt中

我们的目的是实现一个人民币分类的神经网络，编写一个`get_img-info()`方法，读取每一个图片的路径和对应的标签，组成一个元组，再把所有的元组作为list存放到`self.data_info`中，我们需要将标签映射到0开始的整数

我们在`Dataset`类的初始化函数中调用`get_img_info()`方法(`__init__()`)

先要将初始数据划分成训练集，验证集和测试集

* 训练集：设置分类器的参数，训练分类模型
* 验证集，通过训练集训练出多个模型后，找出效果最佳的模型，使用各个模型对验证集数据进行预测，并记录模型准确率，选出效果最佳的模型对应的参数，用来调参
* 测试集，模型预测能力衡量

我们设计一个`Dataset`类，读取图片信息，得到一个列表，列表的每个元素都是一个二元组，表示图片存储的相对路径以及对应的label信息，[`path`,`label`]表示path对应的存储位置图片的label，这是`get_img_info()`的功能，接着我们设计一个函数`__getitem__(self,index)`根据字典的index输出对应index的图片张量

### 训练网络

先构造三个RMBDataset示例

## What is Transform？