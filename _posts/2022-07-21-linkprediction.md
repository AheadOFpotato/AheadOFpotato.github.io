---
layout: post
title: 组内文章 Two-Dimensional Weisfeiler-Lehman Graph Neural Networks for Link Prediction(2022)
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
### ？1维WL
* learn node representation
* link representation: aggregating the pairwise node representations
* 局限性：只学了node representation-->low discriminating power
  <font color=yellow>这边discriminate的是什么</font>

## ？2维WL
* use **links (2-tuples) as message passing units** instead of nodes, and thus can directly obtain link representations
* 优势：discriminate non-isomorphic links
* 