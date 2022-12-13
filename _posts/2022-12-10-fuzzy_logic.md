---
layout: post
title: Fuzzy Logic Based Logical Query Answering on Knowledge Graphs (2022)
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



# 0. Abstract
## task
Answering complex First-Order Logical (FOL) queries on large-scale **incomplete** knowledge graphs (KGs)

## flaws of precvious work
* do not satisfy the axiomatic system of classical logic
* logical operators are parameterized --> need more training data

## this work
* fuzzy logic to define logical operators in a principled and learning-free manner
* 只有 entity 和 relation 的 embedding 是要学的

## performance
### 用 labeled FOL 去训
超sota不少
### 只用 link prediction 去训
comparable performance to those trained with extra data

# 1. Introduction
## challenges of querying KG
* time complexity
  * grow with the query complexity
  * affected by the size of intermediate results

--> difficult to scale to modern KG,e.g. SPARQL

* incomplete KG

--> 无法直接做 searching

## limitations of previous parametric methods
1. 逻辑操作符不规范
2. deep architectures (主要应该是指logical operaters也要训练)--> 难训练

# 3. Preliminaries
# 4. Methodology
## 4.1 Queries and Entities in Fuzzy Space
### Query Embedding
$q$: query; $S_q$: answer set; $\textbf{S}_q\in[0,1]^d$: fuzzy vector

$\Omega$是所有entitiy的集合<font color = yellow>文章中说的elements目前理解是KG中entities的集合</font>，$\{U_i\}_{i=1}^{d}$是$\Omega$的一种划分，fuzzy vector $\textbf{S}_q$中的每个维度就是$U_i\subseteq S_q$的概率
<font color=yellow>U_i是事先划定的？</font>

## Entity Embedding
entity embedding: $\textbf{p}_e\in[0,1]^d$

每个维度代表这个entity属于$U_i$的概率

## Score Function
![1](/_posts/2022-12-13/1.png)