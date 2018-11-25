---
title: Lingo基本操作
date: 2018-09-14 09:56:43
tags: [Lingo,建模]
categories: Lingo
copyright: true
summary_img: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1543068520761&di=a35aeaf7869be06e19ddb8e811e93a14&imgtype=0&src=http%3A%2F%2Fiamwaitress.com%2Fwp-content%2Fuploads%2F2017%2F11%2Flingo-410x220_c.jpg
---

## **前言**

Lingo是一门主要求解非线性规划数学模型的编程软件，记得最初接触Lingo是在阅读[《数学建模教程》](http://product.dangdang.com/24073200.html)一书，该书在第五章主要讲解使用Lingo来解决优化问题，也是在那个时候认识到了Lingo的强大之处。Lingo的使用就好比解决一道简单的数学问题，而你只需要使用Lingo支持的编程规范给其提供充足的已知条件即可，之后会自动使用相关算法为您解答。为了日后更加方便的查询Lingo相关知识，所以将Lingo的基本使用在此记录。

> 关于Lingo的下载及安装问题鄙人已做整理，可参考本篇教程 [Lingo安装](https://blog.csdn.net/Python__Boy/article/details/82055040)

## **Lingo基本运算符**

### **算术运算符**

^：乘方
*：乘
/：除
+：加
-：减

### **逻辑运算符**

在Lingo中，逻辑运算符主要用于集循环函数的条件表达式中，来控制在函数中哪些集成员被包含，哪些被排斥。

|  符号        | 说明  |
| :--------:  | :----:  |
| \#and#   | 且，&     |
| \#or#    | 或，\|\|     |
| \#not#     | 非，! |   
| \#eq#   | 等于，==     |
| \#ne#    | 不等于，!=     |
| \#gt#   | 大于，>     |
| \#ge#   | 大于等于，>=     |
| \#lt#   | 小于，<     |
| \#le#  | 小于等于，<=     |

### ** 关系运算符**

= 、<= 、 >=

## **函数**

### **标准数学函数**

|   函数        | 说明  |
| :--------:  | :----:  |
| @abs(x)    | 绝对值    |
| @sin(x)   |  正弦值，采用弧度制  |
| @cos(x)   | 余弦值   |
| @tan(x)  |    正切|
| @exp(x)    | 指数，$e^x$     |
| @log(x)    |  自然对数  |
| @lgm(x)     | gamma函数的自然对数  |
| @sign(x)   |  x<0返回-1，否则返回返回1  |
| @floor(x)   | 取整    |
| @smax($x_1,x_2,\cdots,x_n$)  | 取($x_1,x_2,\cdots,x_n$) 中的最大值    |
| @smin($x_1,x_2,\cdots,x_n$)  |  取($x_1,x_2,\cdots,x_n$) 中的最小值  |

### **集循环函数**

集循环函数用于遍历整个集，其基本语法如下：

``` Lingo
@function(setname[(set_index_list)[|conditional_qualifier]]:
expression_list);
```

    @function相应于下面罗列的四个集循环函数之一；setname是要遍历的集；set_ index_list是集索引列表；conditional_qualifier是用来限制集循环函数的范围，当集循环函数遍历集的每个成员时，LINGO都要对conditional_qualifier进行评价，若结果为真，则对该成员执行@function操作，否则跳过，继续执行下一次循环。expression_list是被应用到每个集成员的表达式列表，当用的是@for函数时，expression_list可以包含多个表达式，其间用逗号隔开。这些表达式将被作为约束加到模型中。当使用其余的三个集循环函数时，expression_list只能有一个表达式。如果省略set_index_list，那么在expression_list中引用的所有属性的类型都是setname集。
    
#### **@for**

@for函数用来对集中的成员形成约束。

例：产生序列[1,4,9,16,25]

```Lingo
sets:
nums/1..5/: x;
endsets

@for(nums(i): x(i)=i^2);
```

<center>
    <img src="https://gss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/f11f3a292df5e0fede20f83e516034a85edf72d0.jpg"/>
</center>

#### **@sum**

@sum函数返回遍历指定集成员的一个表达式的和

例：求[1,2,3,4,5,6,7]中前五个数的和

```Lingo
sets:
nums/1..7/: x;
endsets

@for(nums(i): x(i)=i);

s = @sum(nums(i) | i #le# 5: x(i));
```

<center>
    <img src="https://gss0.baidu.com/-fo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/91529822720e0cf339223ec40746f21fbe09aa3c.jpg"/>
</center>

#### **@max，@min**

这两个函数分别用于返回指定集成员的一个表达式的最大值和最小值

例：求[1,2,3,4,5,6,7,8,9,10]中前五个数的最大值，后五个数的最小值

```Lingo
sets:
nums/1..10/: x;
endsets

@for(nums(i): x(i)=i);

min_value = @max(nums(i) | i #le# 5: x);
max_value = @min(nums(i) | i #ge# 6: x);
```

<center>
    <img src="https://gss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/cdbf6c81800a19d8eb24f93a3efa828ba61e46ed.jpg"/>
</center>

#### **变量界定函数**

该函数主要是对决策变量做附加限制，一般用于@for函数中，主要有如下四种：

|   函数        | 说明  |
| :--------:  | :----:  |
| @bin(x)    | 限制x为0或1   |
| @bnd(a,x,b) |  限制x取a到b之间的值    |
| @free(x)   | x取实数  |
| @gin(x)  | x取整数  |

#### **说明**

> Lingo中还有其他大量的函数，比如金融函数、概率函数、变量界定函数，由于目前鄙人暂时用不上，所以就暂且不记录了，待需要时再做进一步更新。

## **待更新**







