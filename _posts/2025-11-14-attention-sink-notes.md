---
layout: post
title: "Reading Notes: Attention Sinks in Language Models"
date: 2025-11-14 10:00:00
description: 从 repeated token、over-mixing 和几何视角理解 attention sink，并讨论 diffusion language models。
tags: [transformers, interpretability]
categories: [research-notes]
---

为什么某些 token 会吸引大量 attention？这些阅读笔记整理了几种解释、经验观察，以及在 diffusion language models 中值得继续探索的问题。

## 1. [Interpreting the Repeated Token Phenomenon in Large Language Models](https://arxiv.org/abs/2503.08908)
### overview
* 观察到现象“repeated token divergence”：强大模型难以精确地repeat a single input token when prompted to do so。
* 这个现象跟attention sink相关。利用MI的方法detect attention sink是怎么作用的：initial attention layer识别并mark了sequence中的第一个token，然后后面某一层的single neuron，在第一个token的hidden state上加了较大的值，使得后续attention较大。
* attention sink和repeating token之间的关系：本来要识别第一个token的attention layer没有能够把它和一群identical token分开来，导致它们mark了first token和一群repeated tokens，使得模型在repeated token上的attention过高
![1763095232450]({{ "/assets/img/blog/2025-11-14_attention_sink/" | relative_url }}1763095232450.png)
### some details
* 随着repeated token数量的增长，这个sequence（`[prefix1][prefix2][token][token]...`）的representation(last token, last layer rep)会逐渐趋于`[token]`的representation
* 第一个attention layer是怎么探测sink token的？在llama-2中，llama 2学出了正交的q，k vectors in the first layer's attention head，使得它们会attend to any token other than themselves；而causal mask使得第一个token必须self-attend。而后面找“first token”的mechanism，会找长得像“self attend”的token，而repeat token这个情况下，越到后面的token长得越像first token（由上面一条的理论），所以attention sink难以区分后面的repeat token和BOS。

* 可以利用以上发现，拓展到在repeat token之外的任务上注入新的attention sink。主要利用的就是这个“attend to others”性质。

## 2. [Why do LLMs attend to the first token?](https://arxiv.org/abs/2504.02732)
* research question: Why do LLMs learn such patterns and how are they being used?
* 模型用此机制来避免over-mixing
  * 模型会overmix，as 模型越来越深/context越来越长
    * over-mixing 和 over-smoothing 机制：attention matrices ‘mix’ information, and **repeated mixing** converges to a space that is **uninformative**
    * over-squashing / representational collapse：由于causal mask，模型对于前面的token更为敏感；over long sequences the Transformer tends to destroy information of tokens coming towards the end of the sequence
* attention sink可以control over-mixing
  * 作者提出 attention sink 可能有助于控制 over-mixing；这是一种对该现象的解释。
  * rank 和 representational collapse 是差不多意思：都指的是模型不同token之间的activation的相近程度，这两个现象都是over-mixing的后果，as either the depth or context length growing too much
    * <p align="center">
        <img src="{{ "/assets/img/blog/2025-11-14_attention_sink/" | relative_url }}1763105647627.png" alt="Attention sink analysis figure" style="width: 50%;">
      </p>
  * 衡量对于token i 扰动一下，对于token j的L层表示会有多大的影响
    * <p align="center">
        <img src="{{ "/assets/img/blog/2025-11-14_attention_sink/" | relative_url }}1763105870629.png" alt="Attention sink analysis figure" style="width: 50%;">
      </p>
    * <p align="center">
        <img src="{{ "/assets/img/blog/2025-11-14_attention_sink/" | relative_url }}1763105981831.png" alt="Attention sink analysis figure" style="width: 50%;">
      </p>
  * attention sink是用来control how 以上这个impact的，slow down over-mixing
* 证明在现在的LLM里，attention sink确实有帮助控制over-mixing
  * 对一个token做扰动，看有无`BOS`的时候该扰动的影响。没`BOS`的时候影响大，说明mixing rate较高（？）
    * <p align="center">
        <img src="{{ "/assets/img/blog/2025-11-14_attention_sink/" | relative_url }}1763106947963.png" alt="Attention sink analysis figure" style="width: 50%;">
      </p>
  * 没有BOS以后，attention变得比较smooth
  * 一些head中，attend to BOS意思是不作为
  * attention sink在越长的context和越大的模型里越明显
* 预训练中，attention sink怎么形成的
  * `[bos]`真的重要吗？
    * 如果模型在训练的时候始终在开头见到`[bos]`，那么在inference的时候移走会remove attention sink
    * 如果训练中没有`[bos]`，则模型会在第一个token形成 sink，但会更弱一些

### empirical study of attention sink
[较好的知乎文章](https://zhuanlan.zhihu.com/p/1932066570714215838)：总结了对attention sink的讨论，一句话结论是：Attention Sink来源于Transformer需要Context Aware的Identity Layer，即需要Attention Block根据Context不输出任何变化的能力。

## 3. [What are you sinking? A geometric approach on attention sink](https://arxiv.org/abs/2508.02546)
* attention sink可以锚定representational space
  * 为什么需要这样的anchor：
    * Transformer operations rely on dot products between query and key vectors, making angular relationships crucial for information flow. Reference frames provide fixed geometric anchors that allow tokens to establish relative positions through consistent angular relationships, solving the challenge of maintaining stable
    * Without these anchors, token representations would lack consistent orientation, making reliable encoding of positional and semantic relationships impossible.

<p align="center">
  <img src="{{ "/assets/img/blog/2025-11-14_attention_sink/" | relative_url }}1768309421839.png" alt="Attention sink analysis figure" style="width: 80%;">
</p>

* analyze several architectures and identify three distinct reference frame types, centralized, distributed, and bidirectional, that correlate with the attention sink phenomenon
  * `Centralized Reference Frames`
    * 主要在decoder-only with RoPE里看到
    * Single token acting as a global attractor, like [BOS]
  * `Distributed Reference Frames`
    * 主要在decoder-only + modified PE里
    * Multiple tokens serve as reference points, creating a more flexible coordinate system
  * `Bidirectional Reference Frames`
    * encoder architecture with APE
    * both the beginning token and end tokens serve as reference points, with attention patterns shifting through network depth
> the paper propose the hypothesis of the anchor theory, but do they prove that attention sinks actually work like anchors in practice? e.g., tokens really depend on the relative angles or depend on them during training?

* Information Geometry and the Probability Simplex
  * 也说了为什么attention sink会形成在第一个token：因为第一个token只能attend到自己，所以第一个token的representation在过attention的时候只会scale

* Methods and Results
  * **Geometric and Topological Analysis**: quantify the topological structure of attention networks through persistent homology and Betti numbers, revealing how connectivity patterns evolve across network depth
  * **Information-Theoretic Analysis**:
    * quantified information-geometric properties of attention distributions, particularly how attention sinks affect information flow: `KL Reduction = KL(original) − KL(without sinks)`

## 4. [Attention Sinks in Diffusion Language Models](https://arxiv.org/abs/2510.15731)
和AR模型的主要区别：
1. sink position在整个generation process中会shift
2. 原文报告 AR 模型对 sink removal 更敏感；在其测试的 DLM 中，mask sink 造成的性能下降较小。

* attention sink token的特征：high L2 norm in hidden state activations, low L2 norm in the key projection
---
这篇文章目前只是对open-source DLM做了model-specific的一些分析，还是有可以继续做的空间的，对bi-directional的transformer和causal mask transformer中attention sink的分析感觉还是不太本质。
* 感觉比较本质的事情是，加不加causal mask这件事情对attention sink的影响。比如加causal mask，会有self-attend token/high norm token来区分sink token和非sink token；那不加causal mask，这件事情怎么区分，有没有专门一个子空间/一条circuit来做sink这件事，比如先识别，再扩大其activation？能否predict哪些是sink token。
  * 可以先在训好的开源diffusion上做，看看有无insight
  * 更系统的实验可以按相同语料和architecture去pretrain DLM和ARM，然后观察一些经验现象，比如，attention sink的位置/token/随decoding的变化，尝试解释。
* practically，比如我要做streaming llm这种attention稀疏化，我要怎么确定sink token来做合适的稀疏化
* 一些语句接一堆mask token有点像deepmind那个repeated token的setting，mask token自己的representation是否会和sink token较像，导致sink到mask token里去呢？如果我们在开头/最后接一个BOS/EOS是否会sink过去。

## Open questions


这个事情在autoregressive model和diffusion里真正的区别在哪里？比如模型每次见到的token不只是bos，还有eos。是不是会attention sink to EOS token？

如何鉴定attention sink：semantically meaning less，attentionally attractive

层深 correlates with attention sink 的强度；是不是在looped transformer等扩展层深的模型上有别的表现？


---
关于dense transformer，为什么会有localize效应？比如一个behavior对应一个circuit，或者一个behavior对应一些的neuron，这个事情是为什么会发生的。
