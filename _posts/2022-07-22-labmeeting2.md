---
layout: post
title: lab meeting2
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

# 昊桐师兄：NBFNet in KG
## NBFNet
* bellman-ford algorithm
  * source s-->
  * <font color=yellow>负权重环？</font>
* 推广版本：矩阵形式
  * <font color=yellow>半环性质？</font>
* NN
  * 框架上很像message passing GNN
  * 主要的区别在于indicator借鉴了bellman-ford algorithm

## multi-hop query
并非把operator定义在embedding space上(例如先前的一些工作-->query2box之类)，而是定义在the space of sets of entities
* Representation: (geometric) region --> fuzzy set
* **fuzzy logic**
  * 把真值从 $\{0，1\}$ 泛化到 $[0,1]$
  * 所以从这个 fuzzy logic 可以定义出 **fuzzy set**
* pros and cons：
  * pros:
    * 更符合人类的自然理解
    * 除了projection，其他的operator都是non-parameters
    * handle negation & union
  * cons:
    * fuzzy logic break some logic identity equations-->是否会导致问题


