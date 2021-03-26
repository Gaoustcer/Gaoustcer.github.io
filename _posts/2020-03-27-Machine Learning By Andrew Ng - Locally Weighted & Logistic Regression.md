---
layout:     post
title:      Machine Learning By Andrew Ng- Locally Weighted & Logistic Regression
subtitle:   Machine Learning course
date:       2021-03-25
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - Andrew Ng
---
# Machine Learning By Andrew Ng - Locally Weighted & Logistic Regression

## Outline

* Locally weight regression to fit very non-linear functions
* A probabilistic interpretation of linear regression
* Newton's and logistic regression to classify

$(x_i,y_i)$ i<sup>th</sup> example and $x^i$ is n+1 dimension m is number of example

$x^i=(x_0^i,x_1^i,x_2^i,...,x_n^i)$ $x_0^i=1$

$x=(x_0,x_1,...,x_n)^T$

$\Theta=(\Theta_0,\Theta_1,...,\Theta_n)^T$

$h_\Theta(x)=\Theta^Tx$ -> hypothesis 

$J(\Theta)=\frac{1}{2}\sum_{i=1}^{i=m}(h_\Theta(x^i)-y^i)$ -> loss function

## Locally weighted regression

Target: regression in a curve

* parametric-learning algorithm: linear regression
  * fit in with parameter $\Theta$ without caring about size of dataset
  * can release dataset from memory when training process end
* non-parametric learning algorithm: locally weighted regression
  * need to keep training data set in memory

### Example

$(x^i,y^i)$ Give different weight in different area

Fit $\Theta$ to minimize

$$\sum_{i=1}^{i=m}w^i(y^i-\Theta^Tx^i)^2$$

$w^i$ is the weight function

$w^i=exp(-\frac{(x^i-x)^2}{\delta^2})$

What condition does $w^i$ should fit with?

* if$|x^i-x|$ is small, $w^i$ is close to 1
* if$|x^i-x|$ is large,$w^i$ is close to 0
* $w^i$ should between 0 and 1
* regression with nearby linear model

When n is small, local regression is a good algorithm

## Possibility understanding of Linear regression

Why Square error?

Assume

$$y^i=\Theta^Tx^u+\epsilon^i$$

$\epsilon^i$ ~ $N(0,\sigma^2)$ Gaussian distribution $p(\epsilon^i)=\frac{1}{\sqrt{2\pi}\sigma}exp(-\frac{(\epsilon_i)^2}{2\sigma^2})$

$\epsilon^i $ is IID

请search极大似然估计qwq max-likelihood

## First classification problem

$Y\in \{0,1\}$

### Binary classification 

* It is not a good idea to apply linear regression into classification 
  * if $y<0.5 $ it is class I,else it belongs to class II
  * Some extreme example will influence the linear regression 
* we will introduce logistic regression

### Logistics regression

* Target: output value from $\{0,1\}$
* hypothesis: $g(z)=\frac{1}{e^{-z}+1}$ logistic function or sigmod function
  * to make output between 0 and 1
  * $z=\Theta^Tx$
* Assume $P(y=1|x;\Theta)=h_\Theta(x)$ $P(y=0|x;\Theta)=1-h_\Theta(x)$
  * $P(y|x;\Theta)={h_\Theta(x)}^y{(1-h_\Theta(x))}^{1-y}$
* Maximize general Possibility
  * $L(\Theta)=\prod_{i=1}^{i=m}h_\Theta(x^i)^{y^i}(1-h_\Theta(x^i))^{1-y^i}$
  * $l(\Theta)=lnL(\Theta)=\sum_{i=1}^{i=m}{(y^ilog(h_\Theta(x^i))+(1-y^i)log(1-h_\Theta(x^i)))}$
  * renew parameter
    * $\Theta_j=\Theta_j+\alpha \frac{\partial l(\theta)}{\partial \Theta_j}$
    * we need to maximize likelihood so that $\Theta_j$ should increase!
    * Only global maximize 
    * $\frac{\partial l(\Theta)}{\partial \Theta_j}=\sum_{i=1}^{i=m}(y^i-h_\Theta(x^i))x_j^i$

### Newton's method

* gradient descent takes too much steps
* You have function f and you want to find $\Theta$ that $f(\Theta)+S=0$
  * want to maximize function $l(\Theta)$ and $\frac{d l(\Theta)}{d\Theta}=0$
  * 转化为解方程的牛顿法
* Start from $\Theta^0$
  * 过$\Theta^0$做函数的切线，切线和x轴交点为$\Theta^1$
  * $\Delta=|\Theta_1-\Theta_0|$ height=$f(\Theta^0)$
  * $\dot {f}(\Theta^0)=\frac{height}{\Delta}$
  * $ f(\theta)=\dot l(\Theta)$
  * $\Theta^{k=1}=\Theta^{k}-\frac{\dot l(\Theta^{k})}{\ddot l(\Theta^{k})}$
* When $\Theta$ is a vector
  * $\Theta^{k+1}=\Theta^{k}+H^{-1}\nabla_\Theta l$
  * H is hessian Matrix $H_{ij}=\frac{\partial ^2 l}{\partial \Theta_i \partial \Theta_j}$
  * update for one time is expensive

