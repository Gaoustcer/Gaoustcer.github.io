---
layout:     post
title:      Backpropagation
subtitle:   Machine Learning
date:       2021-04-25
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - Course
---
# Backpropagation

## Gradient Descent

Network parameters $\Theta=\{\omega_1,\omega_2,...,b_1,b_2,...\}$

$\Theta^0->\Theta^1$

$\nabla L(\Theta)=[\frac{\partial L}{\partial \omega_1},\frac{\partial L}{\partial \omega_2},...,\frac{\partial L}{\partial b_1},...]$

$\Theta^1={\Theta}^0-\eta \nabla L(\Theta^0) $

However, millions of parameters..->backpropagation

## Backpropagation

### Chain Rule

#### case 1

$y=g(x)$ $z=h(y)$

$\frac{dz}{dx}=\frac{dz}{dy} \times \frac{dy}{dx}$

#### case 2

$x=g(s)$ $y=h(s)$ $z=k(x,y)$

$\frac{dz}{ds}=\frac{\partial z}{\partial x} \frac{dx}{ds}+\frac{\partial z}{\partial y} \frac{dy}{ds}$

### Def Loss function

$L(\Theta)=\sum_{n=1}^{N}C^n(\Theta)$

$\frac{\partial L(\Theta)}{\omega}=\sum_{n=1}^{N}\frac{\partial C^n(\Theta)}{\omega}$

C is the **distance** between $y^`$ and $y$

