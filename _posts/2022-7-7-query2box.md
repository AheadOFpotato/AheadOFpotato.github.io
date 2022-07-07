# 2022.7.2 Query2box_reading

## abstract

## 1.introduction

KG captures different types of relationships between entities.

First-order logical queries can be represented as Directed Acyclic Graphs (DAGs)(有向无环图)

    -->drawbacks: 1)复杂度expotential in the query size 2)不能回答有missing relation的query
    -->若要解决(2)需要补全KG但那会加剧问题(1)

<img style="float: left;" src="1.png" width="70%" align="left">

another alternative: 把query embed 到一个低维向量空间，使得可以回答query的entities离query比较近。

    -->解决了missing relations的问题
    -->复杂度降低：simply identifying entities nearest to the embedding of the query in the vector space

PROBLEM: prior work embeds a query into a single point in the vector space.

    1 -->answering a logical query requires modeling a set of active entities-->how to effectively model a set with a single point is unclear
    2 -->unnatural to define logical operators (e.g., set intersection) of two points in the vector space
    3 -->can only handle conjunctive queries-->a subset of first-order logic that only involves conjunction (∧) and existential quantifier (∃), but not disjunction (∨). It remains an open question how to handle disjunction effectively in the vector space

QUERY2BOX: an embedding-based framework for reasoning over KGs
capable of handling arbitrary Existential Positive First-order (EPFO) logical queries (i.e., queries that include any set of ∧, ∨, and ∃)
in a scalable manner.

<img style="float: left;" src="2.png" width="30%" align="left">
modeling a set of entities:use a closed region rather than a single point in the vector space
-->use a box (axis-aligned hyper-rectangle) to represent a query

    1.Boxes naturally model sets of entities they enclose
    2.Logical operators (e.g., set intersection) can naturally be defined over boxes similarly as in Venn diagrams
    3.Executing logical operators over boxes results in new boxes, which means that the operations are closed; thus, logical reasoning can be efficiently performed in QUERY2BOX by iteratively(迭代) updating boxes according to the query computation graph

<br>
1.QUERY2BOX can naturally handle conjunctive queries

2.证明把EPFO queries embed到single points or boxes是困难的-->require embedding dimension proportional to the number of KG entities.

3.Solution: transform a given EPFO logical query into a Disjunctive Normal Form (DNF)，i.e., disjunction of conjunctive queries

    Given any EPFO query, QUERY2BOX represents it as a set of individual boxes, where each box is obtained for each conjunctive query in the DNF.

    We then return nearest neighbor entities to any of the boxes as the answers to the query.
--> to answer any EPFO query we first answer individual conjunctive queries and then take the union of the answer entities.

## 2.Futher related work
### most related
approaches for multi-hop reasoning over KGs [Bordes et al., 2013](https://papers.nips.cc/paper/2013/hash/1cecc7a77928ca8133fa24680a88d2f9-Abstract.html)[Das et al., 2017](https://aclanthology.org/E17-1013/)等等

    Crucial difference : we provide a way to tractably handle a larger subset of the first-order logic (EPFO queries vs. conjunctive queries) and that we embed queries as boxes, which provides better accuracy and generalization.

### second line of related
structures embedding: associate images, words, sentences, or knowledge base concepts with geometric objects such as regions

## 3.QUERY2BOX: LOGICAL REASONING OVER KGS IN VECTOR SPACE
要learn什么？

    - embeddings of entities in the KG
    - parameterized geometric logical operators over boxes
输入输出？

    Input：an arbitrary EPFO query q
    -->identify its computation graph(fig.B)
    -->embed the query by executing a set of geometric operators over boxes
    -->Output:Entities that are enclosed in the final box embedding are returned as answers to the query

模型最后能干啥？

    - 准确回答训练集里的问题
    - 能回答非训练集的问题和没见过的logical structure
    - implicitly impute missing relations and answer queries that would be impossible to answer with traditional graph traversal methods

### 3.1 KNOWLEDGE GRAPHS AND CONJUNCTIVE QUERIES

把KG表示成 $\mathcal{G}=(\mathcal{V},\mathcal{R})$
<br>$v\in\mathcal{V}$ 表示entity；$r\in\mathcal{R}$ 表示binary function(二元函数)
<br>r : $\mathcal{V}\times\mathcal{V}\rightarrow\{True,False\}$ ,r表示entities是否成对<font color=yellow>(?成对的意思是direct edge?)</font>

    such binary output indicates the existence of the directed edge between a pair of entities

**conjunctive queries** 是用 $\land$ 和 $\exist$ 的first-order logical queries<font color=yellow>一阶逻辑？</font>的子类。

<img style="float: left;" src="3.png" width="70%" align="left">

其中$v_{a}$是一个不变的锚点, $V_{1},…,V_{k}$是一些有值的bound variables, $V_{?}$是target variable.

The goal of answering the logical query q is to find **a set of entities**  $[q]\subseteq\mathcal{V}$ such that $\mathcal{v}\in[q]$ iff q[v] = True.
<br>[q]: the denotation set (i.e., answer set) of query q

类似于fig.1的dependency graph,是conjunctive query的图表示。其中nodes是variable/non-variable entities,edges是q中的关系.

valid query $\rightarrow$ directed acyclic graph(有向无环图)， 锚点是source nodes, query target $V_{?}$ 是唯一的sink node.

<img style="float: left;" src="1.png" width="70%" align="left">
<img style="float: left;" src="b.png" width="70%" align="left">

从dependency graph可以得出computation graph,其中有两种directed edges:
<br>**Projection 投影**: Given a set of entities $S \in V$, and relation $r \in R$ , this operator obtains $\cup_{v \in S}A_{r}(v) $, where $A_{r}(v) \equiv \{v′\in \mathcal{V} : r(v, v′) = True\}$
<br>**Intersection 交集**: Given a set of entity sets $\{S_1, S_2, . . . , S_n\}$, this operator obtains $\cap_{i=1}^{n}S_i$

computation graph 具体化了寻找sink node的操作

### 3.2 REASONING OVER SETS OF ENTITIES USING BOX EMBEDDINGS

上节已经定义了conjunctive queries,只要按照上面的computation graph 直接执行就可以了.接下来在vector space定义logic reasoning.

步骤: query-->decompose into sequence of logical operations-->execute the operations in the vector space-->embedding of query

**two methodological advances:**
<br>box embeddings --> efficient & reason over sets of entities in the vector space
<br>tractably handle disjunction operator ($\lor$)

#### **Box embeddings.**

$Box_\mathsf{p}\equiv\{\mathsf{v}\in\mathbb{R}^{d}: Cen(\mathsf{p})-Off(\mathsf{p})\preceq v\preceq Cen(\mathsf{p})+Off(\mathsf{p})\}$

相当于一个以cen(p)为中心，以off(p)为范围的一个d维矩形

其中p=(Cen(p),Off(p)), p $\in\mathbb{R}^{2d}$,

#### **Initial boxes for source nodes.**
initial box: (v,0)
