---
layout: post
title: Querying Knowledge via Multi-Hop English Questions
 (2019)
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

# 0. Abstract
## difficulties to KRR
obstacles on the way to make AI based on the knowledge representation and reasoning (KRR):
* inherent difficulty of knowledge specification
* the lack of trained specialists

## current work
用的方法不是 machine learning 范畴的
* based on KALM
* KALM-QA

# 1. Introduction
## what does KRR need
* Knowledge can be described by logical facts and rules, and processed by intelligent knowledge representation and reasoning (KRR) systems.
* such knowledge is hard to extract

## controlled natural languages (CNL)
pros of CNL:
* easy to understand
* usable as knowledge
  as subsets of NLs with restricted grammars and interpretation rules, their meaning can be captured in logic precisely

cons of CNL:
* restrictive grammars could be problematic
  * knowledge authoring
    enabling humans to write in NL that machines could understand
  * knowledge acquisition
    enabling machines to understand the NL that humans write
* CNLs do not assume any kind of background knowledge
  example: *Zoe Saldana appears in Avatar* and *Zoe Saldana is an actress of Avatar* would have different logical representations and a query like *Avatar has which actresses* will yield no answers
* 然而这种缺失的 background knowledge 过多，难以补全--> KALM希望可以把这个步骤做得 scalable and feasible while still relying on CNL

## introduction of KALM
* supports knowledge authoring and simple question answering with very high accuracy compared to the SoTA machine learning approaches
* a sophisticated semantic layer on top of ACE

## contributions
KALM-QA

# 2. The KALM system
* Kalm input: ACE (Attempto Controlled English)
* KALM ensures that semantically equivalent sentences have the same logical representation
* architecture
  ![fig4](/_posts/2022-12-01-faithful/4.png)

## Syntatic parsing
Attempto Parsing Engine (**APE**) as the top level parser
* input and output
    extracts:
    * 每个词的词性
    * grammatical relations

    --> a list of stylized first-order terms: **discourse representation structure, or DRS**

* example:
    ![5](/_posts/2022-12-01-faithful/5.png)

## frame-based parsing
* **frame**
  represents one or more related semantic relationships among entities, where each entity plays a particular role

* example: MOVIE frame
  ![6](/_posts/2022-12-01-faithful/6.png)

  其中bn是同义词编号(based on BabelNet synsets)
  一些role里面有严格的 type constraint
* Ivp (logical valence patterns)
  a lexical unit **&** a set of grammatical patterns, each associated with a frame role
  * A grammatical pattern represents a grammatical relation between the lexical unit and a frame role.
  *  An lvp is applicable to a sentence if
    1.  the sentence contains the lexical unit
    2.  the role-fillers for each frame role can be extracted based on the respective grammatical patterns.
  * example:
    ![7](/_posts/2022-12-01-faithful/7.png)
* we call the extracted relation **a candidate parse**

## Role-filler Disambiguation
一个verb可能对应很多不同的frame，要看两边的名词能不能fit frame 对应的 role filler
![7](/_posts/2022-12-01-faithful/9.png)

## Constructing logical forms

sematic relations extracted via Ivps -->
* facts: unique logical representations (ULR)
* queries: unique logical representation for queries(ULRQ) for questions

*details later*

# 3. The MetaQA Dataset and Multi-Hop Questions
metaQA 对重复名字分辨不出来，里面有很多mislabeled questions

# 4. Constructing a KALM Parser via Structural Learning

这一节讲 frame-semantic parser 逐层的学习结构。

先看用来做 role-filler 的 grammer patterns 是怎么学出来的；再看 Ivp是怎么在 training sentence 上 generate 的

## 4.1 Learning Grammatical Patterns for Role -filler Extraction
用以下的 **prolog** extraction rules:
![10](/_posts/2022-12-01-faithful/10.png)
* grammatical patterns and the corresponding extraction rules: *learned*
<font color = yellow > machine learning 的的那个意义上的learn？</font>
<font color=SkyBlue>halo</font>
* structure-learning process:
  * ACE(一种 CNL) --> DRS
  * pairs in DRS (lexical unit, role-filler) --$^{KALM}$--> extraction rules
    * construction of the library of utility rules
    * reachability reasoning in DRS
    * construction of the actual extraction rules
## utility predicates and rules
<font color=yellow>不是很能理解？</font>

## reachability reasoning in DRS
embed the DRS in a graph structure:
1. For each term in DRS, create a node in the graph.
2. For any pair of nodes n1, n2 that represent the DRS subterms p1, p2 that share a variable, create a directed labeled edge n1 → n2 and back. The label of the edge n1 → n2 is **the position** of the shared variable in p1 and the edge n2 → n1 is labeled with **the position** of that variable in p2.

<font color=yellow>这里为什么用 position 来做 edge label? </font>

# 5. Capturing the Meaning of Multi-Hop Questions in Logic
