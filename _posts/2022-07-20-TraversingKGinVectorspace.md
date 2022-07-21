---
layout: post
title: Traversing Knowledge Graphs in Vector Space(2015)
author: huyi
---
<head>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
</script>
</head>
<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

- [0. Abstract](#0-abstract)
    - [? 解决什么问题](#-解决什么问题)
    - [? recent models](#-recent-models)
    - [? recent models的问题](#-recent-models的问题)
    - [? 这篇文章干了什么](#-这篇文章干了什么)
- [1. Introduction](#1-introduction)
    - [? 模型能做到什么](#-模型能做到什么)
- [2. Task](#2-task)
    - [一些定义：](#一些定义)
  - [Knowledge base completion](#knowledge-base-completion)
- [3. Compositionalization](#3-compositionalization)
  - [3.1 Motivating Example](#31-motivating-example)
  - [3.2 General technique](#32-general-technique)
  - [3.3 Compositional training](#33-compositional-training)

----
<font color=NavyBlue size=5> 这篇文章主要讲的是一个针对path query的形式化的模型，不是很知道必要性在哪里:cry: </font>
----


# 0. Abstract

### ? 解决什么问题

answer compositional questions

### ? recent models

Recent models for knowledge base completion impute missing facts by embedding knowledge graphs in vector spaces
`这篇文章说明了这些model可以用来answer path queries`

### ? recent models的问题

answer path queries时，suffer from cascading errors

### ? 这篇文章干了什么

a `new “compositional” training objective`, which dramatically improves all models’ ability to answer path queries

# 1. Introduction

### ? 模型能做到什么
* compositional training enables us to answer path queries up to at least length 5
* improves upon state-of-the-art performance for knowledge base completion

# 2. Task
### 一些定义：
* **path query:**
  q有一个 anchor entity：s，然后还有一系列的转换关系$p=(r_1,…,r_k)$
* **The answer or denotation of the query,$[\![q]\!]$:**
  the set of all entities that can be reached from s by traversing p
* **candidate answers $\mathcal{C}(q)$:**
  有一个$r_k$（p中最后一个relation）连到它的entity的集合
* **incorrect answers $\mathcal{N}(q)$:**
  \[\mathcal{N}(q)=\mathcal{C}(q)\setminus[\![q]\!]\]

## Knowledge base completion
* **Knowledge base completion (KBC)**：
  task of predicting whether a given edge (s, r, t) belongs in the graph or not.
  * 可以看作一个path query：q = s / r，有一个candidate answer t
  <font color=NavyBlue>显然这边的candidate跟上面说的candidate定义就不一样了</font>

# 3. Compositionalization
--> **compositionalize** existing KBC models to answer path queries
## 3.1 Motivating Example
* 对每个entity，learn一个$x_e\in\mathbb{R}^d$
* 对每个relation，learn一个d*d维的矩阵$W_r$
* 评估：t是不是 query：s/r 的 answer
  ![image.png](https://s2.loli.net/2022/07/21/t9C1gKza4kFYG8E.png)

## 3.2 General technique
![image.png](https://s2.loli.net/2022/07/21/NIRo9WngJzdvj1e.png)

$\mathbb{M}$是membership operator，检验了$x_t$是不是需要求的query的answer。$\mathbb{R}^d\times\mathbb{R}^d\rightarrow\mathbb{R}^d$

$\mathbb{T}$是traversal operator，$\mathbb{R}^d\rightarrow\mathbb{R}^d$

![image.png](https://s2.loli.net/2022/07/21/21iWdN3Ztw5S8Il.png)

## 3.3 Compositional training
minimize the following **max-margin objective**

![image.png](https://s2.loli.net/2022/07/21/dDmtwgi1WuOY4qf.png)