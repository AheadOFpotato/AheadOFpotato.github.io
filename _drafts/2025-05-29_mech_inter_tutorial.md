---
layout: post
title: 大模型机制可解释性tutorial
date: 2025-05-29 15:32:00
description: 整理一下language model这块的入门资料。
tags: LLM-reasoning, mechanism interpretability
categories: sample-posts
---


1. [Arena tutorial](https://arena-chapter1-transformer-interp.streamlit.app/)
2. some libraries of mechanistic interpretability:
   1. [transformerLens](https://github.com/TransformerLensOrg/TransformerLens)
   2. [nnsight](https://github.com/ndif-team/nnsight)
3. Something found in Neel Nanda's personal webpage:
   1. [Neel Nanda's reading list](https://www.alignmentforum.org/posts/NfFST5Mio7BCAQHPA/an-extremely-opinionated-annotated-list-of-my-favourite)
   2. [quick start](https://www.neelnanda.io/mechanistic-interpretability/quickstart)
   3. [Concrete Steps to Get Started in Transformer Mechanistic Interpretability](https://www.neelnanda.io/mechanistic-interpretability/getting-started)
   4. about SAE:
      1. [The ARENA SAE tutorial](https://arena-chapter1-transformer-interp.streamlit.app/[1.3.2]_Interpretability_with_SAEs) 
      2. The [SAELens library](https://github.com/jbloomAus/SAELens) (builds on TransformerLens), useful for both training your own SAEs, and using open source ones
      3. [Gemma Scope](http://huggingface.co/google/gemma-scope), high quality open weight SAEs on every layer and sublayer of Gemma 2 that my team produced
      4. [Neuronpedia](http://neuronpedia.org/), an excellent online tool to look up explanations for different latents (aka features) in open source SAEs
      5. [The dictionary learning library](https://github.com/saprmarks/dictionary_learning)
      6. [the MATS program application guide](https://docs.google.com/document/d/1p-ggQV3vVWIQuCccXEl1fD0thJOgXimlbBpGk6FI32I/edit?tab=t.0), which is also a great guide for people who want to get started with mechanistic interpretability:+1:! a lot of thanks to Neel

---
A lot of interesting research questions are provided in [the MATS application guides](https://docs.google.com/document/d/1p-ggQV3vVWIQuCccXEl1fD0thJOgXimlbBpGk6FI32I/edit?tab=t.0), here is an example regarding reasoning models:

## Understanding thinking models

Eg o1, r1, Gemini Flash Thinking, etc - ie models that produce a really long chain of thought when reasoning through complex problems, and seem to be much more capable as a result. These seem like a big deal, and we understand so little about them! And now we have small thinking models like r1 distilled Qwen 1.5B, they seem quite tractable to study (though larger distilled versions of r1 will be better. I doubt you need full r1 though). 
* This is so understudied that I’m excited about even just rigorous black box investigations here, like seeing what happens when you intervene on the chain of thought
  * For example, when a model solves a hard maths problem by thinking a bunch until it finds the key idea, what’s going on? Did it just “get lucky” and every line was another roll of the dice for finding the key idea? Or was it steadily making progress towards that goal!
  * How deterministic are these models? If you take the first K lines of the CoT and regenerate it, do you get the same output? 
* My mental model is that when o1 is solving a maths problem it’s essentially exploring a “reasoning tree” - at each step in a solution, there’s a bunch of possible next steps. o1 chooses promising ones based on heuristics, and explores that branch of the tree, but with a bunch of reflexes to eg stop and say “that doesn’t sound right” or “I think I’ve gone wrong, let’s backtrack and try another path” or “let’s list out all relevant factors, and hope this helps me choose the next step better”
  * Can you find any evidence for or against this hypothesis?
* An obvious safety strategy is “let’s just read the chain of thought and see if it thinks anything suspicious”. Can you find evidence for or against this being effective?
  * Eg, is the chain of thought faithful to the model’s underlying computation? Can you find evidence of unfaithfulness? What happens if you edit seemingly key steps in the chain of thought?
  * Can you create situations where the model does use suspicious reasoning? What happens if you tell it that its chain of thought is being monitored - does it do the bad behaviour but without suspicious reasoning?
* How well do existing interpretability methods work?
  * Can we probe for anything interesting in the chain of thought?
  * Can we make steering vectors to steer the reasoning? Eg make it backtrack, make it be more creative, etc
  * Do sparse autoencoders tell us anything interesting?
    * You can use the r1 LLaMA 3.1 8B distill and see how well the LLaMA Scope SAEs transfer, you will likely get better results if you finetune the SAEs on the finetuned model activations, as they were trained on base LLaMA 3.1 8B
---
# About Model Diffing

