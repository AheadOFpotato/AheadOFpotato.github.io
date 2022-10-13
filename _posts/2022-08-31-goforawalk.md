---
layout: post
title: GO FOR A WALK AND ARRIVE AT THE ANSWER ———— REASONING OVER PATHS IN KNOWLEDGE BASES USING REINFORCEMENT LEARNING
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


- [2018 ICLR](#2018-iclr)
- [0. ABSTRACT](#0-abstract)
  - [goal](#goal)
  - [popular approach](#popular-approach)
  - [这篇文章提出的方法](#这篇文章提出的方法)
- [1. Introduction](#1-introduction)
  - [goal](#goal-1)
  - [previous way](#previous-way)
  - [这篇文章提出的方法](#这篇文章提出的方法-1)


# 2018 ICLR

# 0. ABSTRACT
## goal
KB completion
## popular approach
* **a popular approach to KB completion is to:**
  infer new relations by combinatory reasoning over the information found along other paths connecting a pair of entities
* **缺陷**
  * Given the enormous size of KBs and the exponential number of paths, previous path-based models have considered **only the problem of predicting a missing relation given two entities**, or evaluating the truth of a proposed triple.
  * Additionally, these methods have traditionally used **random paths** between fixed entity pairs or more recently **learned to pick paths** between them.
## 这篇文章提出的方法
* **MINERVA**
  addresses the much more difficult and practical task of answering questions where the relation is known, but only one entity.
  <br><font color=yellow>不太懂这是什么意思</font>
* **advantage** over path-based way
  Since random walks are impractical in a setting with unknown destination and combinatorially many paths from a start node, we present a **neural reinforcement learning approach** which learns how to navigate the graph conditioned on the input query to find predictive paths.

# 1. Introduction
## goal
  * automated reasoning
  * 具体是如何frame 这个问题的？
    给了一些links，来看一个可以由这些links推断出来的link可不可以被机器推断出来。
    * 例子：We can answer the question “Who did Malala Yousafzai share her Nobel Peace prize with?” from the following reasoning path: Malala Yousafzai → WonAward → Nobel Peace Prize 2014 → AwardedTo → Kailash Satyarthi. Our goal is to automatically learn such reasoning paths in KBs. We frame the learning problem as one of query answering, that is to say, answering questions of the form (Malala Yousafzai, SharesNobelPrizeWith, ?).
## previous way
  * build systems that can learn crisp symbolic logical rules
  * Symbolic representations have also been integrated with machine learning especially in statistical relational learning
    * poor generalization performance
  * Learning embedding of entities and relations using tensor factorization or neural methods
    * **these methods cannot capture chains of reasoning expressed by KB paths**
    <br><font color=yellow>这个问题可以思考一下</font>
  * neural multi-hop model
    operating on KB **paths embedded** in vector space
    * these models take as input a set of paths which are gathered by performing random walks independent of the query relation
    * use the same set of initially collected paths to answer a diverse set of query types (e.g. MarriedTo, Nationality, WorksIn etc.)
## 这篇文章提出的方法
This paper presents a method for efficiently searching the graph for answer-providing paths using reinforcement learning (RL) conditioned on the input question, eliminating any need for precomputed paths.
* 具体方式
  Given a massive knowledge graph, we learn a policy, which, given the query $(entity1, relation, ?)$, starts from $entity1$ and learns to walk to the answer node by choosing to take a labeled relation edge at each step, conditioning on the query relation and entire path history. This formulates the query-answering task as a reinforcement learning (RL) problem where the goal is to take an optimal sequence of decisions (choices of relation edges) to maximize the expected reward (reaching the correct answer node).