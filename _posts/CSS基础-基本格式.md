---
layout:     post
title:      CSS基础
subtitle:   CSS入门
date:       2021-03-24
author:     Haihan Gao
header-img: img/post-bg-swift2.jpg
catalog: true
tags:
    - CSS
    - front-end
---
# CSS基础-基本格式

## 基本概念

### what is CSS

* CSS层叠样式表
* 定义如何显示HTML元素
* 解决内容和表现分离的问题

### 语法格式

选择器+多条声明

```css
p/*选择器*/
{
    color:red;/*属性值*/
    text-align:center;
}
```

### 设置id选择器和class选择器

#### id选择器

用#定义，选择器为#idname，代表样式规则适用于具有id=idname属性的标签

#### class选择器

.表示

```css
.center{text-align:center;}/*定义class选择器*/
p.center/*对所有p元素使用class=“center”属性*/
```

### CSS创建

#### 外部样式表

加上link标签，指向外部css定义文件，下例中从mystyle.css中读取样式文件格式化文档，link标签在文档头部

```html
<link rel="stylesheet" type="text/css" href="mystyle.css">
```

#### 内部样式表

用一对style标签定义

#### 内联样式

每个标签都具有style属性

## 字体和文本属性

### 大小的单位

CSS中必须指定单位，没有默认单位，HTML中默认单位是px

#### 绝对单位

in-英寸 pt-点 pc-皮卡

#### 相对单位

%相对周围文字大小

### font字体属性

* font-size字体大小
* line-height行高->盒子模型
* font-family字体类型
* font-style:italic表示斜体
* font-weight:bold表示粗体

### 文本属性

* text-align在当前容器中的对齐方式

## CSS颜色与背景

### CSS颜色

一共有三种指定颜色的方法：

* 颜色名，是一个字符串
* RGB颜色，rgb(rred,freen,blue)
  * RGBA,制定了颜色的头部都，加上一个alpha参数
* HEX颜色
* HSL值 hsla(hue,satuation,lightness)
  * hue色相
  * saturation饱和度
  * lightness百分比

### CSS背景

#### 背景颜色

* background-color元素的背景色
* opacity不透明度
* 也可以直接在background中使用rgba

#### 背景图像

* Background-image:url
* background-repeat设置背景图像的重复
* background-attachment属性指定背景图像应该是滚动(scroll)的还是固定(fixed)的

## CSS边框与布局

### 边框样式border-style属性

![image-20210316103754292](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210316103754292.png)

### 边框宽度，颜色，各边

* border-width指定4个边框的宽度
* border-color指定边框的颜色
* border-top/right/bottom/left-style指定每个边框属性
* border可以简写边框属性
* border-radius添加圆角边框

## CSS盒子模型

### 外边距

margin属性在任何定义的边框之外，为元素周围创建空间，可以单独指定边框属性margin-top/right/left/bottom,也可以简写属性为margin

### 内边距

padding属性为任何定义在边界内的元素内容周围生成空间

padding-top/right/left/bottom

### 元素的高度与宽度

height属性和width属性用于设置元素的高的和宽度

### CSS框模型

每个HTML元素视为方框，CSS框模型是一个包围每个HTML元素的框，包含外边距，边框，内边距和element，element有height和width两个属性

### 轮廓

轮廓是在元素周围绘制的一条线，由如下属性组成

* outline-style 轮廓样式
* outline-color 
* outline-width
* outline-offset 元素与轮廓之间的空间大小
* outline

## CSS链接，列表，表格

### 链接

* 链接可以使用任何CSS属性，包括color，font-family，background
* 四种链接状态 a:link
  * link 未访问过
  * visited
  * hover 鼠标悬停
  * active 被点击
* text-decoration在链接中删除下划线

### 表格

![image-20210316112641981](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20210316112641981.png)