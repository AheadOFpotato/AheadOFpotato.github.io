---
layout: post
title: GUpdater + MEMIT
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

- [1. Method](#1-method)
- [2. Task and Dataset](#2-task-and-dataset)
  - [Task](#task)
  - [Dataset](#dataset)
- [3. Results](#3-results)
  - [3.1 Triple Extraction: GUpdater](#31-triple-extraction-gupdater)
  - [3.2 Knowledge Insertion: MEMIT](#32-knowledge-insertion-memit)
- [4. Some Other Thoughts](#4-some-other-thoughts)
  - [Flaws of MEMIT](#flaws-of-memit)
  - [Future Work](#future-work)


# 1. Method
![fig](https://github.com/AheadOFpotato/AheadOFpotato.github.io/blob/main/_posts/2023-07-04/pipeline.png)
two-stage method:
1. **Triple Extraction** with [GUpdater](https://aclanthology.org/D19-1265/)
2. **Knowledge Insertion** with [MEMIT](https://arxiv.org/abs/2210.07229)

# 2. Task and Dataset
## Task
  * read texts describing the updates,
  * according to which modify the knowledge graph,
  * and then insert the added triples into GPT.
## Dataset
  modify the family tree dataset, only include one kind of update: change the name of one entity

  ![fig](https://github.com/AheadOFpotato/AheadOFpotato.github.io/blob/main/_posts/2023-07-04/family_tree.png)

  text: *{entity_old} changes his or her name into {entity_new}.*
    ![dataset](https://github.com/AheadOFpotato/AheadOFpotato.github.io/blob/main/_posts/2023-07-04/dataset.png)

# 3. Results
divide the task into two stages
* **first stage:** test the ability of GUpdater to extract the added and removed triples given the texts and the original KGs
* **second stage:** test the performance of MEMIT regarding inserting the added triples into GPT
* **NOTICE:** that MEMIT can only add triples into GPT and is unable to delete triples in GPT, so in the second stage, we do not test its ability to remove triples.
## 3.1 Triple Extraction: GUpdater
* **Result:** the accuracy of added triples is low
* **Reason:** the new triple (like **Dylan** in the showed figure) does not have any link to the other entities in the original KG, so it can be hard to form links between the new triple and the other triples

![table1](https://github.com/AheadOFpotato/AheadOFpotato.github.io/blob/main/_posts/2023-07-04/table1.png)
## 3.2 Knowledge Insertion: MEMIT
* **Result**:
  * MEMIT is not powerful enough to insert memories into GPT.
  * However, bigger models perform better.

![table2](https://github.com/AheadOFpotato/AheadOFpotato.github.io/blob/main/_posts/2023-07-04/table2.png)

# 4. Some Other Thoughts
## Flaws of MEMIT
* A main weak point of
MEMIT is that it cannot remove triples in GPT
* MEMIT cannot deal with one-to-many mapping. To be more specific, triples like (head, relation, ?) can maps to many tail entities, for example, when the relation is “teammate”. However, MEMIT cannot deal with
such situations.

## Future Work
The modeling of “key-value mapping” in MEMIT may be too hard. We may model such relations between
head entities and tail entities in a softer way like works in the field of KG, such as [Q2B](https://arxiv.org/abs/2002.05969) and [FuzzyQE](https://arxiv.org/abs/2108.02390).