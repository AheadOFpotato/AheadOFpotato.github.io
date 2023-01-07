---
layout: post
title: Making Pre-trained Language Models Better Few-shot Learners (2021 danqi chen)
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
GPT-3 few-shot 表现很好，只需要一些 natural-language prompt 和 task demonstrations。本文用一些更小的language model。

## LM-BFF
a suite of simple and complementary techniques for **finetuning** language models on **a small number of annotated examples**.

* prompt-based fine-tuning together with a novel pipeline for automating prompt generation;
* a refined strategy for dynamically and selectively incorporating demonstrations into each context.

## performance
dramatically outperform standard fine-tuning procedures in this low resource setting, achieving up to 30% absolute improvement, and 11% on average across all tasks.

# 1. Introduction
## task setting
task：小样本微调中型的语言模型。

1.  such models can be trained on typical research hardware;

2.  few-shot settings are realistic, as it is generally both easy to acquire a few annotations (e.g., 32 examples) and efficient to train on them;

3. updating parameters typically leads to better performance.

## prompt
* 人工prompt不精准且费力
* auto-prompt
  * introducing automatic prompt generation, including a pruned brute-force search to **identify the best working label words**, and a novel decoding objective to automatically **generate templates using the generative T5 model**

## demonstration
1. randomly sample a single example at a time from each class to create **multiple, minimal** demonstration sets.
2. We also devise a novel sampling strategy that pairs inputs with **similar** examples, thereby providing the model with more discriminative comparisons.

# 3. Problem Setup
## Task formulation