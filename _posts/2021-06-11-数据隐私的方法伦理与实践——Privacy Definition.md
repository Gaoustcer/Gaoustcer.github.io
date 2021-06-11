---
layout:     post
title:      数据隐私的方法伦理与实践——Privacy Definition
subtitle:   Data Privacy
date:       2021-06-11
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Machine Learning
    - Data Privacy
---
# 数据隐私的方法伦理与实践——Privacy Definition

## Dimensions of Private Data Usage

### What to Protect

* Microdata: a set of records containing information on an individual unit
* Macrodata: computed/derive Statistics
* Models and Patterns: models from machine learning

___

Principles:

1. Uninformative principle: 即使有关于被攻击者的背景信息也无法得到发布信息之外的敏感信息
2. 与加密的语义安全类似:即使知道密文也不会泄露报文的额外信息

___

### Disclosure

* Membership disclosure:攻击者可以分别某个个体是否处于数据集中
* Identity disclosure:攻击者可以知道某条属性与特定的人相对应
* sensitive attribute disclosure:攻击者可以知道某条记录没有发布的敏感信息

### Why is difficult

* Privacy: disclosure task
* Utility: Information loss

## Control method

### 不给/部分不给

* Suppress
* Break Linkage
* Monitor the queries: query restriction/auditing

### 给加密的

* secure multiparty computing

### 给不精准的

* Change granularity泛化或聚集
* Change accuracy 交换attribute或加噪音
* Extract features: federated learning

### Suppress

* De-identification   不发布包含敏感信息的条目
* Bucketization 相似的条目分配给一个BID

## K-Anonymity

### basic attempt: de-identification

* Remove/replace personally identifying information(PLL)

  * linking attack: 通过公开发布的一组数据获得特定信息

* Re-identification: 通过一组公开信息重新获得隐私信息


### Attribute Classification

* Identifier attribute: Information that leads to a specific entity
* Quasi-identifier: a tuple of information which can determine a specific entity when they are combined, Maybe you can get the information from other sources
* Confidential or sensitive attributes Information we wish to protect

### Defination

* QI-cluster: all tuples with identical combination of quasi-identifier attribute values in that microdata
* k-anonymity property: 经过k匿名处理的数据每个QI-cluster内部含有k个或多个不同的元组

### Methods to achieving k-anonymity: k-anonymization

#### Generalizatiion

* Replace specific quasi-identifiers with more general values until get k identical values
* Partition ordered-value domains into intervals

___

#### Generalization Types

* for all attributes
  * Full domain generalization
    * 一个数据字段全部泛化为一个值或domain
  * lyengar generalization
  * Cell-level generalization
* Numerical Attributer
  * Predefined hierarchy
  * Computed hierarchy

___

#### Suppression

* 泛化导致过多的信息损失
* 我们需要在实现k匿名的同时减少信息损失

### Attacks Against K-anonymity

* 同质攻击，一个QI-cluster内所有敏感信息都相同
* 背景知识攻击

### P-sensitive K-Anonymity Definition

#### P-sensitive K-Anonymity property

A MM satisfies p-sentive k-anonymity property if it

* 满足k匿名
* 在MM的一个QI-cluster内，敏感信息至少要有p种

## I-Diversity

基本思想:一个QI-Cluster内如果绝大部分或者大比例的元组敏感信息都是某个数值的话，那么猜中敏感信息的概率会增加，也就是K匿名可能丧失意义，这就是probabilistic inference attack的意义

### Entropy I-diversity

* 每一个QI-cluster必须有足够多元的敏感信息，同时敏感信息的分布必须平均
* 每个cluster数据分布情况使用熵衡量
* 在每个QI-cluster里的敏感变量分布的熵至少为log(l)

### Recursive (c,l)-diversity

* 基本思想是出现最多的变量不应该出现的太过于频繁，出现最少的变量不应该出现的过于稀疏
  * $(s_1,s_2,...,s_m)$是敏感值，$q^*$是返回过的QI值
  * 对$(q^*,s_1) (q^*,s_2)...,(q^*,s_m)$的个数进行计数并排序，得到序列$r_1,r_2,...,r_m$
  * (c,l)diversity $r_1<c(r_l+r_{l+1}+...+r_m)$
* Positive Disclosure && Nagative Disclosure
  * Positive Disclosure可以正确地判断个体具有某些敏感属性
  * Nagative Disclosure可以排除个体不具有某些敏感属性

### Bayes-Optimal privacy

* 表中的属性分成不敏感的QI-cluster(Q)和敏感信息S，我们假设知道Q,S的联合分布f
* 如果一个公开泛化的表违反了隐私，那么就意味着表中敏感信息的分布和我们的知识f差异很大
  * 先验知识和后验知识差距很大
* 攻击者的背景知识
  * $$\alpha (q,s)=P_f(t(s)=s|t(q)=q)=\frac{f(s,q)}{\sum_{{s}\in S}f(s,q)}$$
* 表T\*是根据表T泛化得到的，q被泛化为q\*，关于敏感信息的后验知识是
  * $$\beta(q,s,T*)=P_f(t[s]=s|t[q]=q \quad and \quad T* \quad and \quad t\in T*)$$
* 隐私被衡量为观察者的信息增益
  * 如果$\alpha (q,s)\quad and \quad \beta(q,s,T)$对于$\forall \quad q\in Q \quad \forall s \in S$之差都很小，那么说明隐私保护的效果较好

## t-closeness

### distance between Two Probabilistic Distributions

Given Two Probabilistic distributions $P=(p_1,p_2,...,p_m)\quad Q=(q_1,q_2,...,q_m)$,Their distance is measured by

* variation distance $D(p,q)=\sum_{i=1}^m \frac{1}{2}|p_i-q_i|$
* Kullback-leibler divergence $D(p,q)=\sum_{i=1}^{m} p_i\log {\frac{p_i}{q_i}}$

### Earth Mover's Distance

There Constraints guarantee that P is transformed to Q by the mass flow. Once the transportation problem is solved, ther EMD is defined to be the total work

$$D(P,Q)=WORK(P,Q,F)=\sum_{i=1}^m\sum_{j=1}^md_{ij}f_{ij} \tag{1}$$

$$p_i-\sum_{j=1}^mf_{ij}+\sum_{i=1}^mf_{ji}=q_i\tag{2}$$

$$\sum_{i=1}^m\sum_{j=1}^m f_{ij}=\sum_{i=1}^mp_i=\sum_{i=1}^mq_i=1\tag{3}$$

## Defination with background knowledge(攻击者知道什么)

### Background Knowledge

* using Boolean logic sentence to discribe background knowledge
* 稳定的隐私标准应当基于攻击者的信息设置一个上界，预测个人t拥有属性s的概率
* Breach Probability $$max_{t,s} P(t\quad has \quad s|K,D*)<c$$
* D is release data and K is knowledge base

### Specification of Adversarial knowledge

* 困难在于数据发布者不知道攻击者知道什么样的背景知识
* 量化攻击者的知识

### (c,k)-safety

* 知识的临界点k>0一个自信心临界点$c\in [0,1]$发布的数据集D*如果是(c,k)safety如果
  * $$max_{t\in T,s\in S,K\in baseknowledge(k)}Pr(t\quad has \quad s|K,D*)<c$$

### 3D Privacy criterion

* Three intuitive dimensions
* Language $L_{t,s}(l,k,m)$代表攻击者知道的背景知识集合
  * l 目标个体t没有的敏感值
  * k个其它的个体的敏感信息