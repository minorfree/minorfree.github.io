---
title:  "OpenAI Math Dump Hits My Home"
mathjax: true
layout: post
categories: media
date: 2026-10-07 14:35:00 -0400
---

Recent OpenAI [Math dump](https://github.com/openai/math/tree/main/preprints) have solutions to many long standing probems in Math and TCS. In the dump, two problems that I and my friends, notably [Arnold Filtser](https://arnold.filtser.com/), have studied for more than a decade, and published a few papers about this:

1. The $\ell_1$-embedding conjecture for planar graphs. Open AI solution [here](https://github.com/openai/math/blob/main/preprints/Planar-Graph-Metrics-Embed-into-L1-with-Constant-Distortion-September-23-2026/paper.pdf).
2. The $\ell_1$-embedding conjecture for bounded treewidth graphs.  Open AI solution [here](https://github.com/openai/math/blob/main/preprints/L1-Embeddings-of-Graphs-of-Bounded-Treewidth-September-23-2026/paper.pdf).


It is unsettling and hard to swallow. I have not looked at the details yet, and will be doing so in the next few days. On a positive note, I hope to learn new techniques in planar graphs. I always believe that we have not been able to solve these problems because we lack a serious  understanding of planar metrics. Now that they are solved, learning what the serious understanding is exciting.

A clear next prediction (not included among Open AI solution) is a solution of the conjecture that that minor-free graph metrics are embeddable into $\ell_1$ with constant distortion. Using the Robertson-Seymour decomposition, one basically could reduce this conjecture to  bounded treewidth and planar graphs.    At this point, I feel that understanding the two results above are more important than churning out another result. 

Will udpate my understsanding of the two papers above.

I intentially do not mention other big results. What a strange time to be alive.

Updates:


- Dec 07: Read planar L1 Embedding manuscript of Open AI for about 6 hours, gone through the details of the first 12 pages. The proof introduces a very strange model called the **non-crossing column model** of planar graphs, that I have not seen before. I have not yet internalized the model but got a "feel" for it. The writing is so compressed and dense. The expanded version (obtained by querying ChatGPT Astra) has 90+ pages. The "length" of the manuscript released by Open AI is cheated. 