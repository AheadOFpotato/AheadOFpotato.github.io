---
layout: post
title: Prompting - 更好地将语言模型应用到NLP任务 (高天宇)
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

将PLM用于下游任务：
pre-train + fine-tune $\rightarrow$ prompt(提示信息) + demonstration(任务示例)

* prompt
  * prompt是插入到输入样本中的一段文本，因此可以将原始任务转换为（masked）language modeling问题。
  * 例如，假设我们要对影评“No reason to watch”进行情感分类，我们可以在句子中附加一个prompt“It was”，得到“No reason to watch. It was”。这样就可以很自然地认为，LM 会有更高的概率判断为“terrible”而不是“great”

# why prompt?
**less samples**
在标准的“pre-training和fine-tuning”范式中，预训练阶段和下游任务之间的gap可能很大：它们训练目标不同。对于下游任务，我们通常需要**引入新的参数**——例如，对于 BERT 大小的模型和二分类任务，需要额外的一组 1,024 x 2 的参数。而prompting使得下游任务可以采用与预训练目标相同的格式，并且不需要新的参数，如上图所示。对于分类任务，我们只需要设计一个template（“It was”）以及预期的text response（我们称之为label words，例如，图中的正标签词“great”和负标签词“terrible”）。通过缩小两个阶段之间的差距，在特定任务上部署预训练模型就变得容易多了，尤其是对于小样本（few-shot）的情况——当你只有十几个训练样本来完成一项新任务时，很难有效地fine-tune预训练模型和新的task-specific 的参数，但prompting使得这个过程变得顺畅很多。Scao 和 Rush （2021）的研究表明**一个prompt 可能值 100 个常规数据点**，说明prompts可以带来样本效率的巨大提升。
* 两种不同方向
  * 仍然fine-tune
        基于prompt的fine-tuning（关键点是仍然进一步优化参数）被认为是对小语言模型来说更好的few-shot learner途径（“小”指的是拥有数百万而不是数十亿的参数，如 BERT 或 RoBERTa）
  * 不fine-tune了
        固定它们的参数，通过不同的prompt（离散的或soft的，将在后面讨论）将它们应用到不同任务上

# discrete prompts

## 人工prompt
从GPT-1/2开始，通过设计适当的prompt，激活下游任务表现，prompt engineering 影响很大。

## auto prompt / prompt optimization
* 和人工设计的prompt相反，我们也可以生成或优化prompt：Guo等人（2021）表明一种soft Q-learning方法对于promt generation效果很好；AutoPrompt（Shin等人, 2020）建议采用一种基于梯度的搜索（该想法来自Wallace等人, 2019，旨在搜索通用的对抗性触发器，使模型生成一个特定的预测）来找出特定任务的最佳prompt。AutoPrompt的设置的不同之处在于它固定了模型：它假设所有内容都在预训练模型中编码好，我们需要的只是将它们“prompt”出来；另一个原因是 AutoPrompt 还被用于 LAMA（Petroni等人, 2019），这是一项knowledge probing任务，要求不触及模型参数。以下是一个用于情感分类的 AutoPrompt 示例。
* 搜索到的模板显著提高了 LAMA 的性能；它们还在使用完整数据集的情感分类和自然语言推理任务中取得了很高的准确率（不过仍然低于微调的结果）。如果看一下搜索出来的离散（但不再是自然语言形式）prompt，可以找到对一些“trigger tokens”的解释，但其他许多只是特例。目前尚不清楚自动prompt是否真的能帮助LM回忆内部“知识”，还是只是另一种优化方式，是从预训练模型中的“彩票”中挑选“中奖彩票”（对于彩票假设，参见 Frankle和Carbin, 2019）。

# soft prompts
idea：只需在输入序列中放入一些随机向量（与词汇表中的特定word embedding无关）并进行调整，同时固定预训练模型的其他部分

在大模型和大样本情况效果较好

# in-context learning