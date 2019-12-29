---
title: 数据结构与算法python语言实现--递归(recursion)
date: 2019-12-17 22:38:45
categories: 
    - python
    - 数据结构与算法python语言实现
tags: 
    - 算法
    - 基础
mp3: http://xiaoshiyi.top:9000/statics/1-04 Fairy.mp3
cover: http://xiaoshiyi.top:9000/statics/wapper1.jpg
mathjax: true
---
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [说明性的例子](#%E8%AF%B4%E6%98%8E%E6%80%A7%E7%9A%84%E4%BE%8B%E5%AD%90)
  - [阶乘函数](#%E9%98%B6%E4%B9%98%E5%87%BD%E6%95%B0)
  - [二分查找](#%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE)
  - [文件系统](#%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)
- [递归算法的不足](#%E9%80%92%E5%BD%92%E7%AE%97%E6%B3%95%E7%9A%84%E4%B8%8D%E8%B6%B3)
  - [一个低效的计算斐波那契数的递归算法](#%E4%B8%80%E4%B8%AA%E4%BD%8E%E6%95%88%E7%9A%84%E8%AE%A1%E7%AE%97%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E7%9A%84%E9%80%92%E5%BD%92%E7%AE%97%E6%B3%95)
  - [一个高效的计算斐波那契数列的递归算法](#%E4%B8%80%E4%B8%AA%E9%AB%98%E6%95%88%E7%9A%84%E8%AE%A1%E7%AE%97%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97%E7%9A%84%E9%80%92%E5%BD%92%E7%AE%97%E6%B3%95)
  - [Python中的最大递归深度](#python%E4%B8%AD%E7%9A%84%E6%9C%80%E5%A4%A7%E9%80%92%E5%BD%92%E6%B7%B1%E5%BA%A6)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


### 简介

递归是一种技术，这种技术通过一个函数在执行过程中一次或者多次调用其本身，或者通过一种数据结构在其表示中依赖于相同类型的机构更小的实例。  
在计算中，递归提供了用于执行迭代任务的优雅并且强大的替代方案。在数据结构和算法研究中，递归是一种重要的技术。接下来会从三个递归使用例证开始，并给出Python实现。
### 说明性的例子
#### 阶乘函数
一个正整数的n的阶乘表示为n!，它被定义为整数1到n的乘积。如果n = 0，那么按照惯例n!被定义为1。
```python
def factorial(n):
    if n == 0:
        return 1
    else:
        return n * factorial(n-1)
```

#### 二分查找
二分查找是用于在一个含有n个元素的有序序列中有效地定位目标值。这是最重要的计算机算法之一，也是我们经常顺序存储数据的原因。
```python
def binary_search(data, target, low, high):
    """Return True if target is found in indicated portion of a Python list"""
    if low > high:
        return False
    mid = (low + high) // 2
    if target == data[mid]:
        return True
    elif target < data[mid]:
        return binary_search(data, target, low, mid - 1)
    else:
        return binary_search(data, target, mid + 1, high)
```

#### 文件系统
现代操作系统用递归的方式来定义文件系统目录(有时也称为"文件夹")。
也就是说，一个文件系统包括一个顶级目录，这个目录的内容包括文件和其他目录，其他目录又可以包含文件和其他目录，以此类推。
![4-6](http://xiaoshiyi.top:9000/statics/Data%20Structures%20and%20Algorithms%20in%20Python/Chapter4/4-6.png)
报告一个文件系统磁盘空间使用情况的递归函数
```python
import os

def disk_usage(path):
    """Return the number of bytes used by a file/folder and any descendents"""
    total = os.path.getsize(path)
    if os.path.isdir(path):
        for filename in os.listdir(path):
            chlidpath = os.path.join(path, filename)
            total += disk_usage(chlidpath)
    print("{0:<7}".format(total), path)
    return total
```

### 递归算法的不足
虽然递归是一种非常强大的工具，但它也是容易被误用。在这里，我们检查几个问题：一个糟糕的递归实现导致严重的效率低下，并讨论了一些用于识别和避免这种陷阱的策略。
#### 一个低效的计算斐波那契数的递归算法
```python
def bad_fibonacci(n):
    """Return the nth Fibonacci number"""
    if n <= 1:
        return n
    else:
        return bad_fibonacci(n-2) + bad_fibonacci(n-1)
```
不幸的是，这样的斐波那契数公式的直接实现会导致函数的效率非常低。以这种方式计算第n个斐波那契数需要对这个函数进行指数级别的调用:
$$
\begin{eqnarray*}
&c_0& = 1\\
&c_1& = 1\\
&c_2& = 1+c_0+c_1 = 1+1+1 = 3\\
&c_3& = 1+c_1+c_2 = 1+1+3 = 5\\
&c_4& = 1+c_2+c_3 = 1+3+5 = 9\\
&c_5& = 1+c_3+c_4 = 1+5+9 = 15\\
&c_6& = 1+c_4+c_5 = 1+9+15 = 25\\
\end{eqnarray*}
$$
如果遵循这个模式继续下去，我们可以看到，对于每两个连续的指标，后者的数量将是前者的2倍以上。也就是说，$c_4$是$c_2$的两倍以上，$c_5$是$c_3$的两倍以上，以此类推。因此，$c_n>2^{n/2}$意味着bad_fibonacci(n)使得调用的总数是n的指数级。

#### 一个高效的计算斐波那契数列的递归算法

```python
def good_fibonacci(n):
  """Return pair of Feibonacci numbers, F(n) and F(n-1)"""
  if n <= 1:
    return (n, 0)
  else:
    a, b = good_fibonacci(n-1)
    return a+b, a
```

#### Python中的最大递归深度

为了避免无限递归，Python的设计者做了一个决定来限制同时有效激活的函数的总数。这个极限的精确值取决于Python分配，但典型的默认值是1000。如果达到这个限制，Python解释器就生成一个RuntimeError消息：超过最大递归深度（maximum recursion depth exceeded）。

当然，Python解释器可以动态地重置，以更改默认的递归限制。用sys模块可以实现，支持getrecursionlimit函数和setrecursionlimit函数。示例如下：

```python
import sys
old = sys.getrecursionlimit()    # perhaps 1000 is typical
sys.setrecursionlimit(1000000)   # change to allow 1 million nested calls
```

