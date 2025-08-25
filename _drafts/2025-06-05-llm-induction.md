Research Question: How do LLMs extract abstract rules, laws, or concepts from specific cases?

We aim to rigorously define the capabilities and limitations of modern LLMs. What should we realistically expect from them? Can they achieve innovation beyond human-level reasoning? At present, these questions remain unresolved.

To build intuition, let’s start by examining what LLMs can do. We know they fit the training corpus exceptionally well. Beyond the training data, there’s ongoing discussion about their out-of-distribution (OOD) generalization abilities—such as length generalization and compositional generalization.

My intuition is that OOD generalization must stem from some form of induction or abstraction. Only by internalizing certain underlying rules can the model generalize effectively.


# paper list
## generalization
- [ ] `2024.7` Generalization v.s. Memorization: Tracing Language Models' Capabilities Back to Pretraining Data
- [ ] `2024, Cognitive Science` Optimal compression in human concept learning
- [ ] `2024.10, Tegmark` The geometry of concepts: Sparse autoencoder feature structure
- [ ] `2024.6` The geometry of categorical and hierarchical concepts in large language models
## compression and emergence
- [x] `2025.5, Lecun` From Tokens to Thoughts: How LLMs and Humans Trade Compression for Meaning
- [ ] `2025.3` Algorithmic causal structure emerging through compression
- [ ] `2025.5 FAIR, GDM, Cornell, Nvidia` How much do language models memorize?
## compositional generalization
- [ ] `2024.9, Arora`Can Models Learn Skill Composition from Examples
- [ ] `2025.5, KAIST` The Coverage Principle: A Framework for Understanding Compositional Generalization
- [ ] `2024.8, UWM`The Coverage Principle: A Framework for Understanding Compositional Generalization
- [ ] `2024.8 BIGAI` Out-of-distribution generalization via composition: a lens through induction heads in Transformers

## length generalization

# reading notes
## `2025.5, Lecun` From Tokens to Thoughts: How LLMs and Humans Trade Compression for Meaning
### some background and intuitions
* semantic compression: diverse instances $\rightarrow$ abstract representations
* analyze token embeddings?
* trade-off between representational efficiency (compression) and essential semantic fidelity (meaning)

* for humans, concepts enable efficient interpretation, generalization from sparse data, and rich communication; but how do LLMs do such trade-off between *information compression* and *the preservation of semantic meaning*. Do LLMs develop conceptual structures mirroring efficiency and richness of human thought?
```
💭: 压缩的只有concepts这一种吗？有没有hierarchical rule的压缩呢？facts有没有，数据集里尚且能找，rule有没有怎么算？模型有没有真的在内部学到一阶逻辑。
```

* ground concept definitions in established cognitive theory
* a rigorous comparative evaluation of how LLMs and humans balance representational efficiency with semantic fidelity remains a key open area

* three central research questions:
  * <font color=blue>[RQ1]: To what extent do concepts emergent in LLMs?</font>
  * <font color=green>[RQ2]: Do LLMs and humans exhibit similar internal geometric structures within these concepts, especially concerning item typicality?</font>
  * <font color=red>[RQ2]: How do humans and LLMs differ in their strategies for balancing representational compression with the preservation of semantic fidelity when forming concepts?</font>
  * <font color=blue>RQ1</font>: how information is compressed; <font color=green>RQ2</font>: more fine-grained internal structures; <font color=red>RQ3</font>: full framework to compare how LLMs and humans diverge

### Benchmarking against human cognition
focus on classic cognitive benchmark of:
* Rosch (1973): prototype theory - categories are organized around prototypical members rather than strict, equally shared features. 48 items in 8 common semantic categories (e.g., furniture, bird), with prototypicality rankings (e.g., ‘robin’ as a typical bird, ‘bat’ as atypical).
* Rosch (1975): how semantic categories are cognitively represented, provide extensive typicality ratings for a larger set of 552 items across 10 categoties
* McCloskey & Glucksberg (1978): investigate fuzzy boundaries of natural categories, membership often graded rather than absolute; 449 items in 18 categories

### A framework for comparing compression and meaning
Trade off between:
* compress information into efficient representations and；
* preserving the rich semantic fidelity essential for true understanding

### Methods
k-means of token embeddings...