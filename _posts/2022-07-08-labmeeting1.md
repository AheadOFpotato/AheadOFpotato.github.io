---
layout: post
title: lab meeting1
author: huyi
---

# 2022/7/8 组会 (待完善)
## 小娟学姐 RulE:
### Knowledge Graph 简介
![image.png](https://s2.loli.net/2022/07/08/mnAgsV6r5Uc3jyu.png)

KG completion:
- neural(KnowledgeGraphEmbedding)
  - transE: 1t1
  ![image.png](https://s2.loli.net/2022/07/08/RJz8gPVAY2W1lqO.png)
  - TransR: multi-t-multi
- symbolic reasoning
  ![image.png](https://s2.loli.net/2022/07/08/gQXtw4Nv762Vh5O.png)

各有优缺点：
![image.png](https://s2.loli.net/2022/07/08/49bmSqV8ktX7KLD.png)
<font color="yellow">uncertainty为什么对于KGE是一个优势？</font>
<font color="LightSkyBlue">logic rules reasoning其中也有KGembedding</font>

## RulE
rule body-->rule head

### modeling triples: RotateE
- 对KG进行embedding
<font color="yellow">为什么rotate没有带上伸缩？</font>
<font color="yellow">为了把逻辑搞成有序的，需要用rotateK？</font>

aggregation：需要加入pair-wise部分？之后再加三条路……

<font color="yellow">rule跟entity相关？比如说“父亲的配偶是母亲”在不同年代的置信度是不一样的？把entity建模在前面一项</font>

<font color="yellow">为什么后面要加KGE?</font>
<font color="LightSkyBlue">要把entity的信息加进去</font>


![image.png](https://s2.loli.net/2022/07/08/q9KRxUjwZkgl1VW.png)

## 宇新学姐 ParameterAdaptive-GNN
idea：通过图的结构来调整parameter的规模

parameter network --> regularization $\Delta\gamma_{i,k}$ --> classification network

## RNNlogic
naive的做法：将每个可能的rule作为一个candidate rule,然后score每个rule