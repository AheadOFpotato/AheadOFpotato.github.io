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
[toc]
----
# Abstract
### ? 解决什么问题
answer compositional questions
### ? recent models
Recent models for knowledge base completion impute missing facts by embedding knowledge graphs in vector spaces
`这篇文章说明了这些model可以用来answer path queries`
### ? recent models的问题
answer path queries时，suffer from cascading errors
### ? 
