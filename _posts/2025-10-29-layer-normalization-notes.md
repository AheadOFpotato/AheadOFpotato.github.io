---
layout: post
title: "Reading Notes: Layer Normalization in Transformers"
date: 2025-10-29 10:00:00
description: 关于 Pre-LN、Post-LN、训练稳定性和 Transformer 架构改进的阅读笔记。
tags: [transformers, interpretability]
categories: [research-notes]
---

LayerNorm 的位置如何影响 Transformer 的训练稳定性和表示能力？这里整理几种架构设计，以及关于 gradient flow 和 residual amplification 的不同分析。

* some ituitions:
  * **Why can Post-LN be hard to train?** Different analyses highlight large gradients near the output at initialization and sensitivity to parameter changes.
    ![1761794741392]({{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761794741392.png)
  * **why is pre-norm not "expressive" enough**:
    ![1761794621737]({{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761794621737.png)

## architectural improvements

mainly based on gradient norm / latent variance stability

### [RealFormer](https://arxiv.org/abs/2012.11747)：在 Attention scores 中引入残差连接


在attention score的部分加residual

![1761705524608]({{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761705524608.png)
![1761705550834]({{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761705550834.png)

### [Peri-LN](https://arxiv.org/abs/2502.02732)

<div align="center">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761794223348.png" style="width: 40%">
</div>

* if $Norm(Module(Norm(x_l)))$ exhibits near-constant variance $\beta_0$, then

$$
Var(x_{l+1})\approx Var(x_l) + \beta_0
$$

### MIX-LN: UNLEASHING THE POWER OF DEEP LAYERS BY COMBINING PRE-LN AND POST-LN

<div align="center">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761794497911.png" style="width: 80%">
</div>

### The Curse of Depth in Large Language Models

<div align="center">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761794574755.png" style="width: 80%">
</div>

### ReZero is All You Need: Fast Convergence at Large Depth

<div align="center">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761705680009.png" style="width: 40%">
</div>

![1761705693616]({{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761705693616.png)


### [Understanding the Difficulty of Training Transformers](https://aclanthology.org/2020.emnlp-main.463/)

The question: what complicates Transformer training.

* 分析transformer训练不稳定因素：

  * unbalanced gradients are **not** the root cause of the instability of training
  * instead, we identify an **amplification effect** that influences training
    substantially–for each layer in a multi-layer Transformer model, heavy dependency on its residual branch makes training unstable, since it amplifies small parameter perturbations (e.g., parameter updates) and results in significant disturbances in the model output.
  * a light dependency limits the model potential and leads to inferior trained models
* why is Post-LN less robust than Pre-LN

  * gradient vanishing is not the direct reason --> fixing this issue alone cannot stabilize Post-LN training
  * dependency on residual branch plays an essential role: a Post-LN layer has a heavier dependency on its residual branch than a Pre-LN layer
    * strong dependencies of Post-LN amplify fluctuations brought by parameter changes and destabilize the training
* propose Admin: an adaptive initialization method which retains the merits of Pre-LN stability without hurting the performance

  * restricts the layer dependency on its residual branches in the early stage and unleashes the model potential in the late stage

#### gradient vanishing is not the direct cause of training instability

* performance and robustness

  * pre-norm converges in all 15 settings, and Post-LN diverges in 7 out of 15 settings; when Post-LN converges, it outperforms Pre-LN in 7 out of 8 settings.
* only Post-Norm decoder suffers from gradient vanishing problems
* however, when we fix the gradient vanishing problem through hybrid architecture, we still do not solve the unstable training problem

<div align="center">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761707494746.png" style="width: 60%">
</div>

#### instability from amplification effect

<div style="display: flex; align-items: center; width: 100%;">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761720701520.png" style="width: 60%; margin-right: 10px;">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761720685870.png" style="width: 35%;">
</div>

* about viewing the output as ***weighted sum*** of all the residual branches
  * in pre-norm, the residual outputs of different layers are only normalized once, so their weights in the final output (variance) are similar if they are initialized in the same way, and the final results are like average between all the residual outputs
  * however, in the post-norm, the residual outputs of earlier layers are normalized more than once
  * In other words, Pre-LN restricts the network from being too deep (i.e., if it is hard to distinguish $x_i$ and $x_{i+1}$, appending one layer would be similar to doubling the width of the last layer), while Post-LN gives the network the choice of being wider or deeper.

#### Amplification Effect at Initialization

<img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761721036659.png" style="width: 60%;">

post norm relies more on residual branches than pre norm; in the following figure, **post-norm output is more sensitive to weight changes** --> unstable training

<div align="center">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761707494746.png" style="width: 80%">
</div>

#### Admin: adaptive initialilzation method for stable post-norm training

admin adds new parameter $\omega$ and constructs i-th sub-layer as:

$$
x_i = f_{LN}(b_i)
$$

$$
b_i = x_{i-1}\cdot \omega_i + f_i (x_{i-1})
$$

* profiling stage: only conduct forward and record the output variance of residual branches (i.e. calculate $Var[f_i (x_{i-1})]$)
* initialization stage:
  * set $\omega_i = \sqrt{\sum_{j<i}Var[f_j (x_{j-1})]}$
  * sets $\beta_{i,i}^2$ to approximate $\frac{1}{i}$ and ensures an $O(\log N)$ output change thus stablizing training

### [On Layer Normalization in the Transformer Architecture](https://arxiv.org/abs/2002.04745)

* post-norm needs warm-up because the expected gradients near the output layer are large, and large lr can lead to unstable training
* pre-norm do not suffer such problem

<div align="center">
    <img src="{{ "/assets/img/blog/2025-10-29_layernorm/" | relative_url }}1761793812085.png" style="width: 80%">
</div>
<p align="center"> <em>relative gradient norm results in the last paper</em> </p>

## Further reading: Transformer expressivity

* Attention is Turing Complete
* Towards Revealing the Mystery behind Chain of Thought: A Theoretical Perspective
* One-layer transformers fail to solve the induction heads task
* Representational Strengths and Limitations of Transformers
* Transformers, parallel computation, and logarithmic depth
