---
layout: post
author: huyi
---

[toc]

# chap1 Introduction

## 1.1 What is a graph?
$\mathcal{G=(V,E)}$

simple graph，directed graph, directed graph with weighted edges可以由adjacency matrix来表示

    simple graph: 其中的边undirected,且没有自己指向自己的节点)
    directed graph就是adjacency matrix不对称而已

### 1.1.1 Multi-relational Graphs
different types of edges
extend the edge notation to include an edge or relation type $\tau$ , e.g., $(u, \tau, v)\in\mathcal{E}$,对于每一种type $\tau$ 可以有一个adjacency matrix-->adjacency tensor $\mathcal{A}\in\mathbb{R}^{\mathcal{|V|\times|R|\times|V|}}$, 其中 $\mathcal{R}$是 relations的集合

* **Heterogeneous graphs**
nodes也有types, 根据这些type可以把nodes分成各不相交的k个集合：
<br>$\mathcal{V=V_1\cup V_2\cup…\cup V_k}$ where $\mathcal{V_i\cup V_j}=\emptyset$
<br>一般来说，一种relation专门连接两种特定的节点，比如$\tau_i$ 专门连接$\mathcal{V_k} and \mathcal{V_j}$

也有可以连接多种节点类型的relation type

* **Multiplex graphs**
图可以被分成k layers, each layer corresponds to a unique relation.

    Intra-layer edges: 一种专门的edge
    Inter-layer edges: 看一个节点生出来的不同类型的edges

### 1.1.2 Feature Information
node-level attributes that we represent using a real-valued matrix: $\mathsf{X}\in\mathbb{R}^{\mathcal{|V|\times m}}$
<br>$\mathcal{|V|}$个节点，每个节点m维向量
    注：节点顺序和adjacency matrix里面的节点顺序相同
    很少考虑edge在除了types之外还有自己的feature

## 1.2 Machine learning on graphs

problem-driven

分类不可直接沿用 supervised / unsupervised

图机器学习的任务类型：

### 1.2.1 Node classification
the goal is to predict the label $y_u$ associated with all the nodes $u \in V$, when we are only given the true labels on a training set of nodes $V_{train}\subset\mathcal{V}$.

node classification和标准的supervised classification 区别：

        nodes in standard supervised classification: independent and identically distributed
        nodes in graph machine learning: interconnected-->model the relationships between nodes

由于我们可以接触到test nodes的一些信息，所以不完全算unsupervised-->semi-supervised

### 1.2.2 Relation prediction
别名：link prediction, graph completion, relational inference

standard setup：有一堆nodes $\mathcal{V}$, 一堆incomplete set of edges $\mathcal{E}$ \ $\mathcal{E}_{train}$

### 1.2.3 Clustering and community detection
接近于unsupervised

### 1.2.4 Graph classification, regression, and clustering



# chap2 Background and Traditional Approaches

### what's used before the advent of modern deep learning approaches?

针对三种不同的学习任务:

• **node and graph classification tasks**: basic graph statistics, kernel methods
<br>• **relation prediction**: various approaches for measuring the overlap between node neighborhoods,
<br>• **clustering or community detection on graphs**: spectral clustering using graph Laplacians

## 2.1 Graph Statistics and Kernel Methods
**traditional way**: extracting some statistics or features—based on heuristic functions or domain knowledge—and then use these features as input to a standard machine learning classifier

-->introduce some important node-level features and statistics
<br>-->how these node-level statistics can be generalized to graph-level statistics
<br>-->extended to design kernel methods over graphs

==>**GOAL**: introduce various **heuristic statistics** and **graph properties**

### 2.1.1 Node-level statistics and features
以Medicis家族的social network为例：

    what properties or statistics of the Medici node distinguish it from the rest of the graph?
    what are useful properties and statistics that we can use to characterize the nodes in this graph?

**Node degree**

$d_u=\sum\limits_{u\in V}\mathsf{A}[u,v]$表示这个节点伸出来的边的个数

**Node centrality**
$$e_u=\frac{1}{\lambda}\sum\limits_{v\in V}\mathsf{A}[u,v]e_v$$
即centrality和neighbour的centrality 的 weighted sum成正比，其中$\lambda$为一个常数,按照上面的公式,可以发现e是Adjacency matrix的本征矢：
$$\lambda e=\mathsf{A}e$$

还有其他的centrality：
* **betweeness centrality**: how often a node lies on the shortest path between two other nodes
* **closeness centrality**: measures the average shortest path length between a node and all other nodes

**The clustering coefficient**
- -->structural distinction
-->proportion of closed triangles in a node’s local neighborhood
-->how many pairs of nodes there are in u’s neighborhood
![image.png](https://s2.loli.net/2022/07/07/eBDRVzHrtnm3biX.png)
- another way of viewing:
  ego graph: the subgraph containing that node,, its neighbors, and all the edges between nodes in its neighborhood

### 2.1.2 Graph-level features and graph kernels
- 先前的讨论都是node-level的，无论是classification problem还是features 和 statistics.
- 那 **graph-level** classification怎么办？
  这一节的大多方法算在graph kernel methods中
  [graph kernels,Kriege et al. 2020](https://arxiv.org/abs/1903.11835)

**Bag of nodes**
simply aggregating nodelevel statistics

**The Weisfeiler-Lehman kernel**

**Graphlets and path-based methods**