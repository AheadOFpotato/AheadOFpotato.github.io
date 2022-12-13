---
layout: post
title: Logic Embeddings for Complex Query Answering (2021)
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
## task and challenges
**task**: answering logical queries over incomplete knowledge bases

**challenges**:
* **imcomplete KG** --> call for link prediction
* brute force answering of existential **first-order logic queries** is exponential in the number of existential variables

## previous work
### that doesn't support negation
Recent work of query embeddings provides fast querying, but most approaches model set logic with closed regions, so **lack negation**.
<font color=yellow>这里的recent work是query2box？</font>

### that support negation
use densities, triggering drawbacks:
1. only improvise logic
2. use expensive distributions
3. poorly model answer uncertainty

## this work
* support negation
* improve on density approaches:
        1) integrates well-studied t-norm logic and directly evaluates satisfiability,
        2) simplifies modeling with truth values,
        3) models uncertainty with truth bounds

## contribution
* competitively fast and accurate in query answering over large, incomplete knowledge graphs
* outperform on negation queries
* provide improved modeling of answer uncertainty

# 1. Introduction
