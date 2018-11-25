---
title: Lingo解决优化问题
date: 2018-09-14 19:01:07
tags: [Lingo,建模]
categories: Lingo
copyright: true
mathjax: true
---

## 前言

前面，我们已经对Lingo有了一定的了解，但是要想真正的熟悉Lingo在解决优化问题中的强大之处，还需要不断加强相关训练，本文主要是使用Lingo来解决优化问题，该文的主要目的有以下三点：

* 希望能够提升自己对Lingo的相关操作并加强对优化问题的思维模式
* 方便日后对Lingo核心操作的回顾
* 希望每一位到来的朋友能够有所收获

> 若您对Lingo的安装及基本操作不是很了解，可暂且移步：[Lingo安装](https://blog.csdn.net/Python__Boy/article/details/82055040)、[Lingo基本操作](https://blog.csdn.net/Python__Boy/article/details/82224666)

## [优化模型](https://baike.baidu.com/item/%E6%9C%80%E4%BC%98%E5%8C%96%E6%A8%A1%E5%9E%8B/9116053)介绍

优化模型主要有三个基本要素：决策变量、目标函数、约束条件。其一般形式如下：

$$
opt \ \ \ \ f(x) \\
s.t \ \ \ \ h_i(x)=0,\ i=1,2,\cdots,m \\
g_j(x)\leq0,\ j=1,2,\cdots,l
$$

$opt$ 是“optimize”的缩写，表示“最优化”，一般为 $min$ 或 $max$，$f(x)$ 表示目标函数，$s.t.$ 是“subject to”的缩写“受约束于”，$h_i(x), g_i(x)$ 则表示约束条件，其中 $x$ 表示优化模型的决策变量。

## [运输问题](https://baike.baidu.com/item/%E8%BF%90%E8%BE%93%E9%97%AE%E9%A2%98/12734790?fr=aladdin)

### 问题描述

> Question：有三个生产地和四个销售地，其生产量、销售量及单位运费如表所示，求总运费最少的运输方案以及总运费。

<center><img src="https://gss0.baidu.com/-Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/faedab64034f78f0fcf571d074310a55b3191c12.jpg"/></center>

### 问题分析

由题意，我们不难看出优化模型的决策变量是每个生产地向各个销售地运输的货量，即 $s_{ij}$。运输的总费用由各个产地向各个销售地运输所需费用之和，一个产地可以向多个销售地运输货物，一个销售地亦可接受多个产地的货物，所以可知优化模型中的目标函数是运输的总费用，即 $W=\sum^3_{i=1}\sum^4_{j=1}s_{ij}x_{ij}$。除此之外，该目标函数受到两个限制，即优化模型的约束条件：

* 生产地限制：每个生产地的运输量理应小于产生量，$\sum_{j=1}^4s_{ij}\leq a_i$
* 销售地限制：每个销售地接受的货物理应等于销售量，$\sum_{i=1}^3x_{ij}=b_j$

### 优化模型构建

有以上问题分析，为求出总运费最小的方案，我们可以构建该问题的优化模型如下：

$$
min \ \ \ \ \sum^3_{i=1}\sum^4_{j=1}s_{ij}x_{ij} \\
s.t. \ \ \ \ \sum_{j=1}^4s_{ij}\leq a_i \;;\ \sum_{i=1}^3x_{ij}=b_j \ ;\ s_{ij}\geq0 \ ;
$$

### 模型求解

求解的Lingo代码如下：

```Lingo
sets:
supply/1..3/: a;
demand/1..4/: b;
link(supply, demand): c, x;
endsets

data:
a = 30,25,21;
b = 15,17,22,12;
c = 6,2,6,7,
	4,9,5,3,
	8,8,1,5;
enddata

min = @sum(link(i,j): c(i,j) * x(i,j));
@for(supply(i): @sum(demand(j): x(i,j)) <= a(i));
@for(demand(j): @sum(supply(i): x(i,j)) = b(j));
```

### 求解结果

运行如上所示Lingo程序，我们可以得到如下结果：

<center><img src="https://gss0.baidu.com/9fo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/f2deb48f8c5494ee6a0fd73a20f5e0fe99257ee0.jpg"/></center>

通过上图展示，我们可以得到运输的最佳方案以及最小运费161个单位。运输方案图示如下：

<center><img src="https://gss0.baidu.com/9vo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/4e4a20a4462309f7d17476317f0e0cf3d7cad6fe.jpg" width="250"/></center>

### 待续

