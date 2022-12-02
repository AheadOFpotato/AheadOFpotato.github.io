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