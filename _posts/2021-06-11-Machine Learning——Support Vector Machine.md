---
layout:     post
title:      Machine Learning-Support Vector Machine
subtitle:   Machine Learning course
date:       2021-06-11
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - 周志华
---
# 机器学习——支持向量机

## 间隔与支持向量

给定样本集$D=\{(x_1,y_1),(x_2,y_2),...,(x_m,y_m)\}$ $y_i\in\{-1,1\}$，基本的想法是在训练集D的样本空间中找到一个划分超平面，用线性方程表示为$\omega^Tx+b=0$。我们要学习一个$\omega,b$，空间中任意点$x$到超平面$(\omega,b)$的距离表示为

​                                                 $$r=\frac{|\omega^Tx+b|}{||\omega||}\tag{1}$$

我们要求分类超平面满足以下条件，使得训练样本被正确分类

$$\omega^T x_i+b\geq +1,y_i=+1\\ \omega^Tx_i+b\leq -1,y_i=-1 \tag{2}$$

距离超平面最近的几个训练样本点使得(2)中的等号成立，这些训练数据称为**支持向量**，两个异类支持向量到超平面的距离之和称为**间隔**

​                                                                 $$\gamma=\frac{2}{|\omega|}\tag{3}$$

我们希望找到具有最大间隔的划分超平面，数学上看，训练的目标是

$$max_{\omega,b}=\frac{2}{|\omega|}\\ s.t\ y_i(\omega^Tx_i+b)\geq 1\ i=1,2,...,m \tag{4}$$

## 对偶问题

为约束增加拉格朗日乘子$\alpha_i$，得到拉格朗日函数

$$L(\omega,b,\alpha)=\frac{1}{2}||\omega||^2+\sum_{i=1}^m \alpha_i (1-y_i(\omega^Tx_i+b))\tag{5}$$

对$\omega,\alpha$求偏导

$$\omega=\sum_{i=1}^m \alpha_i y_i x_i\\ 0=\sum_{i=1}^m \alpha_i y_i\tag{6}$$

得到对偶问题

$$max_{\alpha} \sum_{i=1}^m\alpha_i-\frac{1}{2}\sum_{i=1}^m\sum_{j=1}^m \alpha_i \alpha_j y_i y_j x_i^T x_j\\ s.t\ \sum_{i=1}\alpha_i y_i=0\\\alpha_i\geq0\tag{7}$$

解出$\alpha$,同时应该满足KTT条件

SMO算法

### 坐标上升算法

目标:求解 $max_{\alpha} W(\alpha_1,\alpha_2,...,\alpha_N)$,选择一个初始的$(\hat{\alpha_1},\hat{\alpha_2},...,\hat{\alpha_N})$,按照如下方式更新

1. i=1
2. 判断结果是否足够精确
3. when i$\leq N$,goto step 3,else goto step 1
4. $\hat{\alpha_i}=\frac{\partial W}{\partial \alpha}$
5. i++
6. goto step2

### 序列最小优化算法

回到SVM问题，我们的目标是优化(7)

* 我们需要选择一对待更新的变量$\alpha_i,\alpha_j$
* 固定除了$\alpha_i和\alpha_j$以外的参数，求解(7)获得更新后的$\alpha_i$和$\alpha_j$

优化两个参数的过程是高效的，是一个单变量二次规划问题，具有闭式解

### 确定偏移项b

对于支持向量$(x_s,y_s)$有$y_sf(x_s)=1$，即(S为所有向量的下标集)

$$y_s(\sum_{i\in S}\alpha_i y_i <x_i,x_s>+b)=1$$,实际上使用所有支持向量求平均值

## 核函数

训练样本不是线性可分的，可以将样本从原始空间映射到更高维的特征空间，使得样本在这个特征空间内线性可分,$\phi(x)$表示x映射到高维空间产生的向量，高维空间中的模型为

$$f(x)=\omega^T\phi(x)+b\tag{8}$$

此时，我们训练的目标是

$$min_{\omega,b}\frac{1}{2}||\omega||^2\\ s.t\ y_i(\omega^T\phi(x_i)+b)\geq 1 \tag{9}$$

对偶问题

$$max_{\alpha} \sum_{i=1}^m \alpha_i -\frac{1}{2}\sum_{i=1}^m\sum_{j=1}^m \alpha_i \alpha_j y_i y_j \phi(x_i)^T\phi(x_j)\\ s.t\ \sum_{i=1}^m \alpha_i y_i=0\\ \alpha_i\geq 0\ \forall i\in\{1,2,...,m\}\tag{10}$$

问题:需要计算$\phi(x_i)^T\phi(x_j)$，由于高维空间中的向量维度可能很高，导致计算向量内积的时间复杂度可能比较大。我们假想一个函数

$$\kappa(x_i,x_j)=<\phi(x_i),\phi(x_j)>=\phi(x_i)^T\phi(x_j)\tag{11}$$

高维向量内积可以通过原始空间中的一个函数计算得到，我们可以用它代替高维向量内积$\kappa$称为核函数。如果我们知道$\phi$的具体形式，那么$\kappa$就可以写出，但是一般比知道$\phi$的具体形式，我们有如下关于核函数的定理。

### 定理(核函数矩阵的性质)

令$\chi$为输入空间，$\kappa$为定义在$\chi\times \chi$上的堆成函数，则$\kappa$为核函数当且仅当对于任意数据$D=\{x_1,x_2,...,x_m\}$核矩阵$K$总是半正定的

$K=\begin{bmatrix} \kappa(x_1,x_1),\kappa(x_1,x_2),\cdots ,\kappa(x_1,x_m) \\ \kappa(x_2,x_1),\kappa(x_2,x_2),\cdots ,\kappa(x_2,x_m)\\ \vdots \\ \kappa(x_m,x_1),\kappa(x_m,x_2),\cdots,\kappa(x_m,x_m) \end{bmatrix} \tag{12}\ $

### 性质(核函数的性质)

* 核函数的线性组合也是核函数
* 核函数的直积也是核函数
* 若$\kappa_1$为核函数，则对于任意函数$g(x)$ $\kappa(x,z)=g(x)\kappa_1(x,z)g(z)$也为核函数

## 软间隔与正则化

线性可分可能导致过拟合，我们需要运行支持向量机在一些样本上出错，引入软间隔的概念

### 软间隔

硬间隔需要要求每个样本均分类正确，但是实际上我们运行部分样本分类错误，这就是**软间隔**。同时我们认为，最大化间隔的同时，不满足约束的样本应该尽可能少，我们改写优化目标

$$min_{\omega,b} \frac{1}{2}||\omega||^2+C\sum_{i=1}^m l_{0/1}(y_i(\omega^T x_i+b)-1)\tag{13}$$

C是一个常数，$l_{0/1}$是衡量损失的函数，它的定义如下

$l_{0/1}=\begin{cases} 1,if\ z<0 \\ 0,otherwise\end{cases}\tag{14}$

实际上，由于$l_{0/1}$的性质不够好，我们用其它一些函数替代这个损失函数，称为替代损失

* hinge损失 $l_{hinge}(z)=max(0,1-z)$
* 指数损失 $l_{exp}(z)=exp(-z)$
* 对率损失 $l_{log}(z)=\log (1+exp(-z))$

### 松弛变量

由于可能存在样本不满足约束条件，引入松弛变量$\xi_i$,问题变为

$$min_{\omega,b,\xi_i}\frac{1}{2}||\omega||^2+C\sum_{i=1}^m \xi_i\\ s.t\ y_i(\omega^T+b)\geq 1-\xi_i\\ \xi_i\geq 0,i=1,2,\cdots,m \tag{15}$$

### 支持向量回归

## 补充

### 为什么引入对偶问题

* 对偶问题更易求解
* 更自然地引入核函数

原问题涉及两个变量的优化，$\omega,b$

$$min_{\omega,b}J(\omega)=min_{\omega}\frac{1}{2}||\omega||^2\\ s.t \ y_i(X_i^T\omega+b)\geq 1\tag{16}$$

将这个问题一般化

$$min_{x\in R^n} f(x)\\s.t \quad c_i(x)\leq 0 \ i=1,2,\cdots, k\\ h_j(x)=0 j=i,2,\cdots, l\tag{17}$$

引入拉格朗日函数，为了将有约束条件的问题求解转化为没有约束条件的问题求解

$$L(x,\alpha,\beta)=f(x)+\sum_{i=1}^k \alpha_i c_i(x)+\sum_{j=1}^l\beta_j h_j(x)\tag{18}$$

假设我们固定拉格朗日乘子，$\alpha_i,\beta_j$，并且$\alpha_i\geq 0$,定义条件最大值

$$\Theta_p(x)=max_{\alpha,\beta,\alpha_i\geq 0}L(x,\alpha,\beta)=max_{\alpha,\beta,\alpha_i\geq 0}(f(x)+\sum_{i=1}^k\alpha_i c_i(x)+\sum_{j=1}^l \beta_j h_j(x))\tag{19}$$

简化以下这个问题，考虑只有一个不等式约束的优化问题$P_1$

$$min \ f_0(x)\\ s.t\quad f_1(x)\leq 0\tag{P1}$$

最优值表示为 $p^*=inf\{f_0(x)|f_1(x)\leq 0\}$ 假设D为全局定义域，定义一个G$$G=\{(f_1(x),f_0(x))|x\in D\}$$

重写最优值$$p^*=inf\{t|(\mu,t)\in G,\mu\leq 0\}$$