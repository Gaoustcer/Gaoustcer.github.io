---
layout:     post
title:      Machine Learning By Andrew Ng- Linear regression
subtitle:   Machine Learning course
date:       2021-03-24
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - Andrew Ng
---
# Machine Learning By Andrew Ng- Linear regression

## Basic Concept

### Regression Problem

input x output y

Data set $(x_i,y_i)\quad i=1,2,3,...,m$

Supervised Learning: Dataset with data and character 

Output: hypothesis function to predict

### layout of the algorithm

* find s set of function(hypothesis) $y=\Theta_0+$$\sum_{i=1}^{i=n}\Theta_i\times x_i $
  * def $x_0=1$
  * $\Theta=(\theta_0,\theta_1,...,\theta_n)^T$-parameter
  * x-input $x=(x_0,x_1,...,.x_n)^T$ m is the number of Training Data set
  * y is output 
  * $(x^{(i)},y^{(i)})$ is the i<sup>th</sup> training sample
* target: choose $\Theta$ to make h(x) close to y
  * minimize $\frac{1}{2}\times\sum_{i=1}^{i=m}{(h_\Theta(x^{i})-y^{i})^2}$

## gradient descent

* If we use linear regression ,there will not have local min
* $\Theta_j=\Theta_j-\alpha \frac {\partial J(\Theta)}{\partial \Theta_j}$
  * $\alpha$ is the learning rate
  * $\frac{\partial J(\Theta)}{\partial \Theta_j}=\sum_{i=1}^{i=m}(h_\Theta(x^{i})-y^i)x_j^{i}$ Assume there is m training data
  * It doesn't have local min
  * You have to try to set the value of $\alpha$
* batch gradient descent - use all training data as a batch
  * disadvantage: update parameter is costly for reviewing all training data

## Stochastic gradient descent

* initial from a random point
* improve according to one training data
* $\Theta_j=\Theta_j-\alpha (h_\Theta(x^i)-y^i)x_j^i$
* might not reach global min but it is close to global min

## Prove: In linear regression model we can get global min

$\nabla J=[\frac{\partial J}{\partial \Theta_0},\frac{\partial J}{\partial \Theta_1},...,\frac{\partial J}{\partial \Theta_m}]^T$

$A=$$$\left\{\begin{matrix} a_{11} & a_{12} &... &a_{1n}\\...\\a_{n1} & a_{n2} &...& a_{nn}\end{matrix}\right\}$$ $tr(A)=\sum_{i=1}^{i=n}a_{ii}$

F is a function $R^{n\times n}->R$ F(A)

$\nabla _A F(A)=[\frac{\partial F}{\partial a_{ij}}]_{n\times n}$

F(A)=tr(AB) $\nabla F=B^T$

$F(A)=tr(AA^TC)$ $\nabla _A F=CA+C^TA$

$J(\Theta)=\frac{1}{2}\sum_{i=1}^{i=m}(h_\Theta(x^{(i)})-y^{(i)})^2$

Training sample matrix $X=[x^{(0)},x^{(1)},x^{(2)},...x^{(m)}]^T$

$J(\Theta)=\frac{1}{2}(X\Theta-y)^T(X\Theta-y)$

$\nabla _\Theta J(\Theta)=\frac{1}{2}\nabla _\Theta(\Theta^TX^T-y^T)(X\Theta-y)=X^TX\Theta -X^Ty$

We get the **normal equation** $X^TX\Theta=X^Ty$