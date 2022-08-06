---
layout: post
title: Neural-Symbolic Models for Logical Queries on Knowledge Graphs
author: huyi
---

<head>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
    </script>
</head>

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({ tex2jax: { skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'], inlineMath: [['$','$']] } });
    </script>
</head>

[toc]

# Abstract
### 问题
Answering complex first-order logic (FOL) queries on knowledge graphs

### 传统解决方案
* Traditional symbolic methods traverse a complete knowledge graph to extract the answers
<font color=yellow>traverse a complete knoeledge graph是什么意思？</font>
<font color=NavyBlue>应该是说只能应用在complete KG上</font>

* Recent neural methods learn geometric **embeddings** for complex queries.
  --> reasoning process比较难做(GQE,query2box之类的)

### 提出了GNN-QueryExecuter
