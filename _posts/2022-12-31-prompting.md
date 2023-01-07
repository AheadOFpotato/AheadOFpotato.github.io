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
GPT-3 简单地将训练集中的一些随机样本与实际查询（query，在本示例中为“cheese ⇒”）连接起来，而且由于预训练模型已经学会了从上下文中捕获模式，并且Transformers 的 self-attention 使这些实例之间可以逐token进行比较，in-context learning的效果出奇地好。GPT-3 论文将其归为“元学习（meta-learning）”，认为在阅读大量无监督文本后，语言模型可以“培养广泛的技能和模式识别的能力”。作者认为在预训练期间“有时会在单个序列中嵌入重复的子任务”，类似于in-context learning的范式。后续工作进一步完善了使用demonstration的方式：Gao等人（2021）Liu 等人（2021）认为不应该随机抽取一些样本，采用**和查询近似的demonstration**可以显着提高性能；Lu等人（2021）表明，即使是demonstration的**顺序**也很重要，并提出了一种确定“最佳”顺序的方法。

虽然in-context learning只有在不能微调模型时才是“必要”的，并且当训练样例数量增加时很难泛化（因为模型的输入长度有限），研究如何更好地使用demonstration（即如何进一步压缩 LM 学到的“元知识”）以及哪些预训练目标和数据可以提高in-context 能力，可能会进一步帮助我们了解预训练模型的内部工作机制。

# 校准语言模型
prompting很赞，但它也会从预训练语料库带来bias。例如，在零样本情感分类设置中，给定“N/A”作为输入，GPT-3 倾向于预测为“positive”而不是“negative”，而本应该分配50/50的概率给这两个相反的标签（赵等人，2021 ）。另一个问题是同一对象的不同表示（例如，“computer”和“PC”）可能会竞争概率质量，导致任务标签上的分布不理想（Holtzman 等，2021）。赵等人（2021）和Holtzman 等人（2021）给出的解决方案是校准（calibration）：对带偏token进行补偿，把他们校准为无偏状态。