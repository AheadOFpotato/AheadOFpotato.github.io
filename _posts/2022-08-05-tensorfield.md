---
layout: post
title: Tensor field networks---- Rotation- and translation-equivariant neural networks for
author: huyi
---

<head>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
    </script>
</head>

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({ tex2jax: { skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'], inlineMath: [['$','$']] } });
    </script>
</head>

[toc]

# 0. Abstract
* tensor field neural networks:  locally equivariant to **3D rotations**, **translations**, and **permutations** of points at every layer
# 1. Motivation
* CNN的平移不变性：识别目标放在图的哪里都可以识别得到
# 2. Related work

# 3. Group representations and equivariance in 3D

## 3.1 Permutation equivariance

## 3.2 Translation equivariance

## 3.3 Rotation equivariance

# 4. Tensor field network layers

## 4.1 Point convolution

### 4.1.1 Spherical harmonics and filters

### 4.1.2 Combining representations using tensor products

### 4.1.3 Layer definition

## 4.2 Self-interaction

## 4.3 Nonlinearity

# 5. Demonstrations

## 5.1 Geometry: shape classification

## 5.2 Physics: vectors and tensors in classical mechanics

## 5.3 Chemistry: toward geometrically generating molecular structures

# 6. Future work