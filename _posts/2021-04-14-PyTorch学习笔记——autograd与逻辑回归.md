---
layout:     post
title:      PyTorch学习笔记——autograd与逻辑回归
subtitle:   PyTorch
date:       2021-04-16
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - PyTorch
---
# PyTorch学习笔记——autograd与逻辑回归

## 自动求导

pytorch自动求导离不开计算图，搭建好计算图，就可以使用`torch.autorgad`自动求导得到所有张量的梯度

### torch.autograd.backward()

```python
torch.autograd.backward(tensors, grad_tensors=None, retain_graph=None, create_graph=False, grad_variables=None)
```

* tensor用于求导的向量
* retain_graph是否保存计算图，动态图默认每次反向传播之后都会释放计算图
  * 对一个张量多次求导，可以将这个属性设置为true
* create_graph创建导数计算图，用于高阶求导
* grad_tensor多梯度权值，多个张量混合计算梯度时，设置每个张量的权重

关于grad_tensor，有这样一个例子

假设$y_0=y_0(x_1,x_2),y_1=y_1(x_1,x_2)$我们将$y_0,y_1$组合成一个张量$y=(y_0,y_1)$定义$\frac{\partial y}{\partial x_1}=\omega_1\times \frac{\partial y_1}{\partial x_1}+\omega_0\times \frac{\partial y_1}{\partial x_1}$,求导的时候需要设置权重向量，使用以下方法

 ```python
loss=torch.cat([y0,y1],dim=0)
grad_tensors=torch.tensor([1,2])
 ```

设置权重分别为1，2

### torch.autograd.grad()

```python
torch.autograd.grad(outputs, inputs, grad_outputs=None, retain_graph=None, create_graph=False, only_inputs=True, allow_unused=False)
```

* output用于求导的向量
* input需要梯度的张量
* create_graph创建导数计算图，用于高阶求导
  * 为了求二阶导，需要设置此属性为true
* retain_graph保存计算图
* grad_outputs多梯度权重计算

注意：(1)每次反向传播求导，计算的梯度不会自动清零，进行多次迭代计算梯度，梯度会在上一次基础上叠加，可以使用`w.grad.zero_()`清零梯度 (2)依赖于叶子节点的节点，require_grad属性默认为True (3)叶子节点不能进行inplace操作 inplace操作改变后的值和原先内存共享一块地址空间

如果在反向传播之前inplace改变了叶子的值，执行backward会报错

## Logistic regression

Assume $$y=\frac{1}{1+e^{-z}}\quad z=\omega x+b$$

$$\ln {\frac{y}{1-y}}=\omega x+b$$

