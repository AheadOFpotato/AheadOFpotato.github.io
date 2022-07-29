---
layout: post
title: labeling trick_GNN多节点表示学习理论
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

# 0. Introduction
## Previous Work
以前更多的是通过 *聚合* 的方式来表示多节点。如：两个节点的link prediction

-->作者：这样的方法有问题，且有更好的办法——labeling trick

### Heuristic methods for link prediction
1-hop
* **CN：common neighbour**
* **preferential attachment(PA)**
  * 更popular的更容易被认识

2-hop
* **Adamic-Adar(AA)**:带权重的 common neighbour
  * 一个 popular 的 common neighbour 对 这两个节点之间是否形成link 影响很小。
  * 应用场景：都认识明星不大会让A和B更有可能成为朋友

高阶
* **Katz index**
  * 计算所有X到Y的path，给越长的path越小的权重
* **Rooted PageRank**
  * 以固定概率从节点一步一步往外走，看最后达到稳态从x走到y的概率

![image.png](https://s2.loli.net/2022/07/29/hHgoWZftG96mEU1.png)

#### Heuristic methods的缺陷
![image.png](https://s2.loli.net/2022/07/29/PWlDd2h9GT6VtKv.png)

## GNN review
* Input-output
* layers
  * aggregation layer
  * set aggregation layer (pooling)
 ![image.png](https://s2.loli.net/2022/07/29/YaDczJK1kg3ldCT.png)

# 1.Motivation
![image.png](https://s2.loli.net/2022/07/29/XsWjvmrTAOFqPGz.png)

v2,v3由于在图中的位置非常对称，所以embedding是一样的。若通过对node embedding的aggregation来表示link，那么 r(v1，v2) 和 r(v1,v3) 是一样的。这显然不对！

# 2. Definition
## Set Isomorphism
![image.png](https://s2.loli.net/2022/07/29/4ctwj3VKWzpxgvC.png)

* adjacency tensor: 邻接张量
  * 前两个维度可以用来存adjacency matrix
  * 第三个维度可以用来存别的，比如link的feature之类
* 两个节点如果在图中有对称位置则是同构的；link等多个节点的set也有同构之说

## Structural Representation
-->可以用来 identify 同构这件事情

说明了一个等价性的事情：

    两个节点集同构 <--> 两个节点集的 structural representation 一样

* **node-most-expressive GNN**：可以学到structural node representations
* Question：学到了node-most-expressive GNN之后，可不可以做structural link prediction？
* 不可以！（之前的例子就说明了这个问题）

  ![image.png](https://s2.loli.net/2022/07/29/q6oYUOVv7gPlGR3.png)

  ![image.png](https://s2.loli.net/2022/07/29/wH4QxmXg1WNsKbq.png)

# 3. Labeling trick