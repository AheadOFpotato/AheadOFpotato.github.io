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

#
* bellman-ford algorithm
  * source s-->
  * <font color=yellow>负权重环？</font>
* 推广版本：矩阵形式
  * <font color=yellow>半环性质？</font>
* NN
  * 框架上很像message passing GNN
  * 主要的区别在于indicator借鉴了bellman-ford algorithm