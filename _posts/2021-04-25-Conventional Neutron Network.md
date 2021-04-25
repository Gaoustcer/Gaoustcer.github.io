---
layout:     post
title:      Conventional Neutron Network
subtitle:   Machine Learning Theory
date:       2021-04-25
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - Course
---
# Conventional Neutron Network

## Why CNN for Image

* Image is classified by patterns and some patterns are much smaller than the whole image
* same pattern appear in different
* reduce parameter
* subsampling will not change the image

## The whole CNN

* convolution
* Max pooling
* flatten

## Convolution

* Image$6\times 6$
* several Filter(numbers in filter is the parameter)卷积核，parameter被学习得到$3\times 3$
  * Filter放在image左上角，做内积
  * 挪动filter(stride)
  * 得到$4\times 4$matrix
  * matrix中元素的最大值，代表最符合这个pattern的部分
  * 其它filter对图片做原来的操作，得到多个matrix，称为一个Feature map
* colorful image
  * filter是三维张量
* 内积的过程相当于消除一部分参数的fully connected network，得到的卷积值相当于一个神经元
  * shared weight

## Max Pooling

* We get a $4\times 4$ matrix, we divide it into 4 part: $2\times 2$ each part choose max element to represent it. And finally we get a $2\times 2$ Matrix
* This output will become another Convolution input,后面卷积层的输入要考虑前面张量层的尺寸

### CNN in keras

Only have to modify network structure and input format

```python
mode12.add(Convolution2D(25,3,3,input_shape=(1,28,28))) #convolution layer 25 is filter number and size is 3*3
mode12.add(MaxPooling(2,2))  #maxpooling reduce 2*2 matrix
mode12.add(Convolution2D(50,3,3))
```

* After convolution, we get $25\times 26\times 26$
* After max pooling, we get $25\times 13\times 13$
* After convolution, we get $50\times 11\times 11$,the filter is $25\times 3\times 3$ 25 is decided by fore layer
* After max pooling,we get $50\times 5\times 5$
* Flatter to get a vector size is 1250,and it will be an input of fully connected network

### What does CNN learn

Output of k<sup>th</sup> filter is $11\times 11$ matrix A=$(a_{ij}^k)_{11\times 11}$ degree of the activation of k<sup>th</sup> filter is $a^k=\sum_{i=1}^{11}\sum_{j=1}^{11} a_{ij}^k$

We want to find an input x to max $a^k$:$x^*=argmax_xa^k$

each filter should be able to find character of part of image(pattern)

Output layer $x^*=argmax_x y^i$ is the not the number image(Not trained parameter).then we add some constrain $x^*=argmax_x(y^i-\sum_{i,j}|x_{ij}|)$ regularization

cnn will exaggerates what it sees

## CNN in Text process

* every word is represented by a vector
* the sentence contains several word and they become a matrix(word embedding) assume the matrix is $R^{d\times s}$
* Convolutional feature map assume a filter is $R^{d\times m}$ we will get a vector $R^{m-s+1}$,there exists special relationship between different columns

