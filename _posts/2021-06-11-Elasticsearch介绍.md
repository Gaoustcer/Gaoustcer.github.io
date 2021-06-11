---
layout:     post
title:      Elasticsearch介绍
subtitle:   Elasticsearch
date:       2021-06-11
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - Elasticsearch
    - Search model
---
# Elasticsearch介绍

**Author** 高海涵

资料参考：知乎，官方文档

## What is elasticsearch

* Elasticsearch是一个全文搜索引擎，可以快速地存储，搜索和分析海量数据
* ES建立在Lucene全文搜索引擎库上，相当于Lucene的一层封装
* 通过pip可以安装elasticsearch包，这是python API

## Elasticsearch的基础概念

### Cluster

一个集群，含有多个节点，可以理解为运行ES进程的一组物理机

### Node

ES节点，可以是ES服务进程，Node和cluster基于分布式数据库模型

### Index

ES索引，是一个具有相同结构的文档集合，一个集群中可以包含多个索引，索引相当于数据库

索引一个文档，表示把文档存储到索引(数据库)中，使得其可以被检索或者查询，相当于往一条记录中插入关键字

倒排索引，传统数据库为特定列增加一个索引，ES使用的是倒排索引的数据结构

### Document

Index内的单条记录称为Document，Document的集合称为Index，Document采用json格式，对于同一个index中的Document，不需要scheme相同，但是**相同的scheme可以提高搜索效率**

### Fields

JSON结构的document包含了许多字段，document由多个字段组成，相当于MySQL中的字段

下面这张表对比一下关系数据库和Elasticsearch的基本概念

| Relation DB | Elasticsearch    |
| ----------- | ---------------- |
| Database    | Index            |
| Table       | Type(已经被废除) |
| Row         | Document         |
| column      | Field            |

### shard

分布式系统设计的初衷就是为了克服单一节点导致的存储资源、计算资源不足的问题，我们将索引拆分为分布在多个节点上的元素，称为shard，Elasticsearch的一个重要的任务是管理分片和平衡分片在每个节点上的负载

## Elasticsearch数据结构

Elasticsearch支持模糊查询，利用索引快速实现模糊查询，并且可以根据评分过滤掉大部分结果，只返回评分高的给用户，如果关键字不是那样准确也能返回相关的结果

写入数据到Elasticsearch会进行分词，分词是快速模糊查询/相关性查询的基础

下面这张图表明了索引和查询的关系

```mermaid
graph TD
A[文档数据库]--索引-->B[索引表]
B--查询-->A
A--包含-->文档ID
A--包含-->文档内容
B--包含-->Term(分词结果)
B--包含-->Count(出现次数)
B--包含-->D(Documentid:position)
```



## Elasticsearch搜索排序，相关性和评分机制

### 什么是相关性

相关性是推荐搜索结果的一个指标，对一个查询请求，对每个文档生成`_score`字段，字段的值代表文档可能满足用户要求的程度

### 怎么根据文档内容和查询请求计算得到相关性

Elasticsearch相关性算法被定义为检索词频率/反向文档频率

* 检索词频率——一个检索词在文档中出现的概率越高，相关性越高
* 反向文档频率——我们针对文档建立了一些索引，如果一个检索词在索引中出现的概率高，那么相对其它检索词，它对相关性的贡献度较低，这样做可以避免文档中通的检索词出现，比如`and` `or`等
* 字段长度准则——举个例子，同样一个词Apple,对于文档A，Apple出现在文档的标题中，但是对于文档B，Apple出现在文档的文本内容中，显然文档A相对文档B更可能是我们希望的结果

## Python控制Elasticsearch

**注意，执行下列代码前需要启动elasticsearch**

### 创建/删除Index

```python
from elasticsearch import Elasticsearch
#创建index 
es = Elasticsearch()
result = es.indices.create(index='news', ignore=400)
print(result)
```

print返回json格式的数据

```json
{
    "acknowledged":true,
    "shards_acknowledged":true,
    "index":"news"
}
```

acknowledge字段表示创建index成功，创建index使用的参数ignore表明如果创建函数返回的状态码是400就忽略这个错误不会报错，程序不会产生运行时异常

删除index只需要将`create`改成`delete`即可，`ignore=[404]`忽略index不存在导致的运行时异常

### 插入/更新/删除数据

Elasticsearch插入的数据基本类型是字典类型，创建所以调用的create方法，index参数表明的是索引名称，body表示索引内容，id是唯一标识

```python
data = {'title': '美国留给伊拉克的是个烂摊子吗', 'url': 'http://view.news.qq.com/zt2011/usa_iraq/index.htm'}
result = es.create(index='news', id=1, body=data)
print(result)
```

返回值为json格式

```json
{
    "_index":"news",
    "_type":"_doc",
    "_id":"1",
    "_version":1,
    "result":"created",
    "_shards":{
        "total":2,
        "successful":1,
        "failed":0
    },
    "_seq_no":0,
    "_primary_term":1
}
```

`result`字段为`created`,表明数据插入成功，以`_`开头的都是系统字段

create方法插入数据需要我们自己决定唯一的id是什么，我们也可以通过index方法插入数据不需要自己指定id，插入的数据内容属于`doc`属性

更新数据需要调用`update`方法，指定`index`，`id`，和更新后的`doc`内容

Elasticsearch函数相当于返回一个数据库查询的会话

```python
data = {
 'title': '美国留给伊拉克的是个烂摊子吗',
 'url': 'http://view.news.qq.com/zt2011/usa_iraq/index.htm',
 'date': '2011-12-16'
}
# 注意update时的doc与create的doc有所不同
result = es.update(index='news', body={'doc':data}, id=1)
print(result)
#更新操作也可以使用index操作实现
es.index(index='news',body=data,id=1)
```

删除数据，只需要使用delete方法并且指定删除数据的id，返回json数据`result`字段为delete即为删除成功

### 查询数据(这一部分我们后面开一个单独的部分讲解，查询是elasticsearch的核心技术)

#### Query查询

查询上下文，需要计算文档是否匹配，还需要计算文档相对于其它文档匹配度多高，**需要计算得分**_score

#### Filter查询

仅仅关心文档是否与查询匹配，**不计算匹配度**，性能更高

#### 实例

我们先插入几条新数据

使用关键词`index`进行查询

```python
result=es.search(index='news')
```

返回json格式的文本

```json
{'took': 2, 'timed_out': False, '_shards': {'total': 1, 'successful': 1, 'skipped': 0, 'failed': 0}, 'hits': {'total': {'value': 4, 'relation': 'eq'}, 'max_score': 1.0, 'hits': [{'_index': 'news', '_type': 'faq', '_id': '4HFm1W4BMbliRpYyaAqj', '_score': 1.0, '_source': {'title': '美国留给伊拉克的是个烂摊子吗', 'url': 'http://view.news.qq.com/zt2011/usa_iraq/index.htm', 'date': '2011-12-16'}}, {'_index': 'news', '_type': 'faq', '_id': '4XFm1W4BMbliRpYyaQoE', '_score': 1.0, '_source': {'title': '公安部：各地校车将享最高路权', 'url': 'http://www.chinanews.com/gn/2011/12-16/3536077.shtml', 'date': '2011-12-16'}}, {'_index': 'news', '_type': 'faq', '_id': '4nFm1W4BMbliRpYyaQoc', '_score': 1.0, '_source': {'title': '中韩渔警冲突调查：韩警平均每天扣1艘中国渔船', 'url': 'https://news.qq.com/a/20111216/001044.htm', 'date': '2011-12-17'}}, {'_index': 'news', '_type': 'faq', '_id': '43Fm1W4BMbliRpYyaQo6', '_score': 1.0, '_source': {'title': '中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首', 'url': 'http://news.ifeng.com/world/detail_2011_12/16/11372558_0.shtml', 'date': '2011-12-18'}}]}}
```

返回的结果在`hits`字段中，total表明的是返回的记录总数，`_score`表明查询得分

我们要对文本进行全文检索，支持DSL语句进行查询，json查询语句中match表明使用的是全局搜索

```python
import json
dsl = {
    'query': {
        'match': {
            'title': '中国领事馆'
        }
    }
}
es = Elasticsearch()
result = es.search(index='news', body=dsl)
print(json.dumps(result, indent=2, ensure_ascii=False))
```

查询的结果会进行自动分词，之后根据评分算法推荐

```json
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 2,
      "relation": "eq"
    },
    "max_score": 3.7446182,
    "hits": [
      {
        "_index": "news",
        "_type": "faq",
        "_id": "43Fm1W4BMbliRpYyaQo6",
        "_score": 3.7446182,
        "_source": {
          "title": "中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首",
          "url": "http://news.ifeng.com/world/detail_2011_12/16/11372558_0.shtml",
          "date": "2011-12-18"
        }
      },
      {
        "_index": "news",
        "_type": "faq",
        "_id": "4nFm1W4BMbliRpYyaQoc",
        "_score": 0.60291106,
        "_source": {
          "title": "中韩渔警冲突调查：韩警平均每天扣1艘中国渔船",
          "url": "https://news.qq.com/a/20111216/001044.htm",
          "date": "2011-12-17"
        }
      }
    ]
  }
}
```

第一条记录含有两个关键词，第二条仅含有一个，所以第一条评分更高

## Elasticsearch倒排索引

ES中每个字段都是被索引的，只要插入到ES中的数据都会对其进行分词并建立索引，这样做的好处是可以帮助我们根据关键词定位出现关键词的文档

### Inverted Index

索引实际上是文本和文档ID组成的映射，相当于一个字典，字典的key是索引的词，value是出现key的文档ID

我们向数据库插入三个文本

1. Winter is coming
2. Ours is fury
3. Ths choice is yours

ES进行分词并建立索引，得到下列索引表(只列出了部分单词，实际上对于每个分词的结果都需要建立表中的一项)

| 单词   | 词频 | 出现单词的文档ID列表 |
| ------ | ---- | -------------------- |
| choice | 1    | 3                    |
| coming | 1    | 1                    |
| ours   | 1    | 2                    |

为了提高查找效率，实际上表中的每一项都是按照某种顺序排好序的，这样就可以通过二分查找的方式快速定位表中某一项，实际上和mysql中B+树索引没有什么不同

显然，对于大规模的文档，直接将索引表搬入内存的开销是不可接受的，需要进行进一步处理，即建立**词典索引**，通过词典所以可以找到term在index table中的大致位置，我的理解是在index table的基础上再次做一次哈希操作得到一个较小的表，实际上index table存储在磁盘上，只需要将term index加载到内存中

### Finite State Transducers

构建一个Tire前缀树，方便我们根据term快速找到Term对应的词典位置，这里的假设是Term Table中是按照单词的字典序组织元组的，即term字典序在前的tuple在表的前面，举个例子，我们查询的词是mop,moth,pop,star,stop,top，他们的编号为0...5我们建立如下的状态转换图

![image-20210430115909410](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210430115909410.png)

每个边代表一个字母。找到一个单词就需要经过图上的某些边，某些边上具有权重，将权重相加得到的顺序即为单词在表中的位置

实际上，树保存的是词的前缀而不是每个词本身，确定词的前缀之后定位其存储的硬盘block再在block内查找即可。

**模糊查询**查询的是search字段和目标文本之间的**编辑距离**，ES4.0之后的版本可以直接通过一个有限状态转换机得到指定编辑距离内的单词

### Posting Lists

Posting Lists是包含某个term的ID集合，如果每个ID都采用一个integer标识，会导致Posting Lists过大。ES将数据存储到不同的segment中，segment相当于分片的分片，segment之间会定期合并，每个segment被分配唯一的id

### Frame of reference

进行组合查询的时候经常需要对查询的结果进行交集并集运算，需要分别查出posting lists并做集合运算。posting list中的编码都是有序的，为了减少存储空间，id会使用增量编码。

原本的posting list中，每个元素是文本的id，我们将其转化为其相对于前一个id的增量，这样每个元素存储所需的字节可以减少，第一个id前的id默认为0

`[73, 300, 302, 332, 343, 372]`转化为`[73, 227, 2, 30, 11, 29]`

### Roaring Bitmaps

相当于缓存，缓存查询次数比较多的查询请求对应的结果

