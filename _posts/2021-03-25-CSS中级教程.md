---
layout:     post
title:      CSS中级教程
subtitle:   CSS进阶一
date:       2021-03-24
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - CSS
    - front-end
---
# CSS中级教程

## Display属性

* 决定如何/是否显示元素
* 每个HTML元素都有一个默认的display值，具体取决于它的元素类型

### 块级元素(Block element)

块级元素总是从新行开始，并且占据全部的可用宽度

### 行内元素(inline element)

内联元素不从新行开始，仅占用所需的宽度

* span
* a
* img

### Display:none

用于隐藏和显示元素，script脚本默认，每个元素都有默认的display值，可以覆盖之

## CSS选择器

### 元素选择器

* 直接选择文档的元素
* 根据标签选择某个HTML元素
* 元素选择器又称为类型选择器，类型选择器匹配文档语言元素的类型，类型选择器匹配文档树中该元素类型的每一个实例

### 选择器分组

* 同时使用一个规则到多个文档中元素
* 可以使用通配符匹配
* 对声明分组，需要在各个声明最后使用分号

### 类选择器

* 指定一个class类名称
* 类名前有.号
* 类选择器可以结合元素选择器 p.important会匹配class属性包含important的所有p元素
* 多类选择器，可以设置class包含一个class列表
* 将两个类选择器链接到一起，仅可选择同时包含这些类名的元素

### ID选择器

* ID选择器只能选择一次
* 不能使用ID词列表

### 属性选择器

* 根据元素的属性和属性值选择元素，下面可以将包含title这个属性的元素设置为红色

```html
[title]
{
color:red;
}
<h2 title="Hello world">Hello world</h2>
<a title="W3School" href="http://w3school.com.cn">W3School</a>
```

* 选择特定的标签

## CSS导航栏

### 水平导航栏

* 设置行内列表
* 浮动到容器的某个位置