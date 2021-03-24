---
layout:     post
title:      Introduction to Deep Learning
subtitle:   Machine Learning course
date:       2021-03-24
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
---
# Brief Introduction to Deep Learning

## Ups and Downs of Deep Learning

* Perceptron
* Perceptron has Limitation
* Multi-layer perceptron
* Backpropagation
* Hidden layer is good enough
* RBM initialization
* GPU
* speech

## Three Steps of deep learning

* Function Set: Neutral network
  * Fully Connected Feedforward Network
  * Set Weight and Bias
  * Input and output are both vector. Function Set has different weight and bias
  * input/output/hidden layer
  * Matrix Operation
  * Add the last layer with soft max
  * How many hidden layer and how many neurons in hidden layer

$$\left [\begin{matrix}w_{11}& w_{12}&...& w_{1n}\\w_{21} & w_{22} &...&w_{2n}\\...&...&...&...\\w_{m1}&w_{m2}&...&w_{mn}\end{matrix}\right]=Weight$$ $$X={(x_1,x_2,...,x_n)^T} B={(b_1,b_2,...,b_m)^T} Y={(y_1,y_2,...,y_m)^T}$$

We will have following equation

$$Y=\sigma (Weight \times X+B)$$

* goodness of Function

output of the network $y=(y_1,y_2,...,y_n)$ ideal output $Y=(y1,y2,...,yn)$

Cross Entropy(y,Y),and min cross Entropy L=$\sum_n{C_n} C_n=crossentropy(y_n,yn)$

* optimize the model- gradient decent