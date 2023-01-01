---
layout: post
title: Reading Wikipedia to Answer Open-Domain Questions
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
**machine reading at scale**: tackle open-domain question answering using Wikipedia tackle opendomain question answering using Wikipedia

## challenge
1. document retrieval (finding the relevant articles)
2. machine comprehension of text (identifying the answer spans from those articles)

# 1. Introduction
## why wikipedia instead of knowledge base?
* KBs are easier for computers to process but too sparsely populated for open-domain question answering.(Miller,2016)

* Wikipedia contains up-to-date knowledge that humans are interested in. It is designed, however, for humans – not machines – to read.

## machine reading at scale (MRS)
不依赖freebase这种图结构

our approach is generic and could be switched to other collections of documents, books, or even daily updated newspapers.