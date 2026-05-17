---
title:  "Two New Preprints: Balanced Separators for (Fat) Minor-Free Graphs"
mathjax: true
layout: post
categories: media
---

The [graph minor structure theorem](https://en.wikipedia.org/wiki/Graph_structure_theorem) by Robertson and Seymour, and balanced separators are two central algorithmic tools in minor-free graphs. I personally like thinking about balanced separators. I wrote [about it 3 years ago](https://minorfree.github.io/sep-theorems/) on this blog. A few months ago, we gave the first [linear time algorithm](https://minorfree.github.io/SepLinear/) for finding such a separator. Now I write about it again, focusing on two recent preprints.

1. [A Separator for Minor-Free Graphs Beyond the Flow Barrier.](https://arxiv.org/abs/2605.05494v2)
2. [Coarse Balanced Separators in Fat-Minor-Free Graphs](https://arxiv.org/abs/2604.11318), joint with [Édouard Bonnet](https://perso.ens-lyon.fr/edouard.bonnet/), [Marcin Pilipczuk](https://www.mimuw.edu.pl/~malcin/) and [Michał Pilipczuk](https://www.mimuw.edu.pl/~mp248287/).

I am sure this is not the last time I write about separators; more will be coming.

Fat-minor-free graphs are a relatively new concept and quite interesting. I will say more about them below.

---

## 1. The Balanced Separator Conjecture for Minor-Free Graphs

In 1990, [Alon, Seymour, and Thomas](https://web.math.princeton.edu/~nalon/PDFS/Publications/A%20separator%20theorem%20for%20non-planar%20graphs.pdf) (AST) proved their landmark result that \\( K_h \\)-minor-free graphs have a balanced separator of size \\( O(h^{3/2} \sqrt{n}) \\). (In this section, I will not treat \\( h \\) as constant so the dependence of the separator's size on \\( h \\) will be given explicitly.) They conjectured that the size of the separator should be the clean bound \\( O(h\sqrt{n}) \\).

***

**AST Separator Conjecture:** \\( K_h \\)-minor-free graphs have a balanced separator of size \\( O(h \sqrt{n}) \\).

***

They also observed that the bound in the conjecture would be: Expander graphs of degree-3 have clique minors of size \\( h = \Theta(\sqrt{n}) \\) and balanced separators of size \\( \Theta(n) \\).

The history of the progress on this conjecture is a bit messy. Kawarabayashi and Reed [claimed a solution](https://ieeexplore.ieee.org/document/5670816) but the precise bound on the size of their separator is \\( O(h\sqrt{n} + f(h)) \\) where \\( f \\) is a very fast-growing function in the graph minor structure theorem. So it is only \\( O(h\sqrt{n}) \\) for very small values of \\( h \\). On the other hand, it is clear that AST is interested in the regime where \\( h \\) is relatively large, in which the bound by Kawarabayashi and Reed is vacuous.

I wrote about another relatively simple algorithm by [Plotkin, Rao and Smith](https://minorfree.github.io/sep-theorems/) which gives a balanced separator size \\( O(h\sqrt{n \log n}) \\). There is an annoying \\( \sqrt{\log n} \\) factor, which makes the bound inferior to the original AST bound for small \\( h \\).

There is another amazing line of work on constructing balanced separators for \\( K_h \\)-minor-free graphs using the duality between concurrent flow and sparsest cut. In this line of work, the paper to read is [Korhonen and Lokshtanov](https://arxiv.org/abs/2308.04795), a beautiful paper, giving a clear presentation of how the flow-cut duality helps in finding balanced separators. In a nutshell, the lesson is this: if the flow-cut duality gap is \\( \beta \\), then \\( K_h \\)-minor-free graphs have balanced separators of size 
$$O(h\beta \sqrt{n}).$$

I included the detail of this in [Section 4 of this preprint](https://arxiv.org/abs/2605.05494v2). Of course if one can show \\( \beta = O(1) \\), then one proves the AST conjecture. However, for general graphs, \\( \beta = \Omega(\log n) \\), and for \\( K_h \\)-minor-free graphs, \\( \beta = \Omega(\log h) \\). The recent paper on padded decomposition by [Filtser and Conroy](https://arxiv.org/abs/2504.00278) (which I also [wrote about previously](https://minorfree.github.io/PaddedSolved/)) showed that one can achieve the optimal \\( \beta = O(\log h) \\). This means one can construct a balanced separator of size \\( O(h\log h \sqrt{n}) \\), which is pretty close to the AST conjecture.

However, the \\( \log h \\) factor is inherent in this type of technique due to the lower bound \\( \beta = \Omega(\log h) \\). As beautiful as it is, this technique cannot give a proof of the AST conjecture. So the question I am interested in is whether one can get below the "flow barrier" bound \\( O(h\log h \sqrt{n}) \\). And as you might well guess, the answer is yes: [the first preprint](https://arxiv.org/abs/2605.05494v2) mentioned above gives a balanced separator of size 
$$O(h\sqrt{\log h} \sqrt{n}).$$

The \\( h\sqrt{\log h} \\) factor above mysteriously coincides with the same factor in the [sparsity bound](https://minorfree.github.io/minor-sparsity/). I think it is just a coincidence and nothing else. To get this bound, I add a new twist, namely a low-diameter decomposition, to the [same old framework](https://minorfree.github.io/sep-theorems/) for balanced separators. If you are curious, check [it](https://arxiv.org/abs/2605.05494v2) out.

---

## 2. Balanced Separators in Fat-Minor-Free Graphs

Michał introduced me to the fat-minor-free graph stuffs in a [Dagstuhl workshop](https://www.dagstuhl.de/seminars/seminar-calendar/seminar-details/25212) that I co-organized. Fat-minor is a central concept in a so-called [coarse graph theory](https://arxiv.org/pdf/2305.07456). There are many interesting and ambitious conjectures in the theory. The most famous one is the existence of a quasi-isometry to minor-free graphs (Conjecture 1.1 in the linked preprint). Unfortunately, many, if not all, of these conjectures have been recently disproved, including the famous conjecture. But not all are lost. And one of which is the existence of a good coarse balanced separator, the topic discussed here.

Let me introduce the notion of a \\( d \\)-fat clique minor. A clique minor \\( K_h \\) of a graph \\( G \\) refers to a collection of vertex-disjoint connected subgraphs that have pairwise edges between them; this collection is called a \\( K_h \\)-minor model. A graph is \\( K_h \\)-minor-free if it does not have a \\( K_h \\)-minor model. Easy peasy.

There is a more complicated way to define a \\( K_h \\)-minor model of \\( G \\). Let \\( B \\) be a map that maps each vertex \\( v \\) in \\( K_h \\) to a connected subgraph \\( B_v \\) of \\( G \\) and each edge \\( e \\) in \\( K_h \\) to a connected subgraph \\( B_e \\) of \\( G \\). The map \\( B \\) realizes a \\( K_h \\)-minor model if:

1. whenever \\( v \\) is an endpoint of \\( e \\), \\( B_v \\) and \\( B_e \\) share at least one vertex.
2. otherwise, subgraphs in \\( \{B_v\}_{v \in V} \cup \{B_e\}_{e \in E} \\) are vertex disjoint.

![K4-fatminor](/assets/figs/K4-fatminor.svg)

Figure 1: A \\( 2 \\)-fat minor of \\( K_4 \\) in a graph \\( G \\). Picture generated by Gemini.

This somewhat complicated definition is naturally generalizable. Given an integer \\( d \ge 1 \\), a map \\( B \\) realizes a \\( d \\)-fat \\( K_h \\)-minor if:

1. whenever \\( v \\) is an endpoint of \\( e \\), \\( B_v \\) and \\( B_e \\) share at least one vertex.
2. otherwise, subgraphs in \\( \{B_v\}_{v \in V} \cup \{B_e\}_{e \in E} \\) are *distance at least* \\( d \\) in \\( G \\).

When \\( d = 1 \\), a \\( d \\)-fat \\( K_h \\)-minor is also a \\( K_h \\)-minor. Things are interesting when \\( d \ge 2 \\). For example, a \\( 2 \\)-fat minor is closely related to an induced minor. Things are more complicated when \\( d \ge 3 \\), which is the central regime in coarse graph theory.

We say that a graph class is *fat-minor-free* if every graph in the class has no \\( d \\)-fat \\( K_h \\)-minor for a constant \\( d \\) and \\( h \\). Fat-minor-free graphs are very rich: a complete graph on \\( n \\) vertices excludes a \\( K_2 \\) (yes, \\( K_2 \\)) as a \\( 2 \\)-fat minor. This is because one cannot have a map \\( B \\) mapping two vertices of \\( K_2 \\) to two vertex-disjoint subgraphs of distance \\( 2 \\): every pair of distinct vertices in \\( K_n \\) has distance exactly 1. But richness comes at a cost: it's hard to understand this class.

A question we ask is a separator theorem for fat-minor-free graphs. Of course, we can hope to have a balanced separator with a sublinear number of vertices, as the complete graph \\( K_n \\) does not have any. But we could hope for a balanced separator that contains a small number of balls of constant radius. Specifically, the question Michał asked is:

***

**Fat-Minor Separator Conjecture:** For any two given integers \\( h, d \in \mathbb{N} \\), there exist constants \\( c, r \in \mathbb{N} \\) such that every \\( n \\)-vertex graph \\( G \\) that excludes \\( K_h \\) as a \\( d \\)-fat minor has a balanced separator that can be covered by \\( c \sqrt{n} \\) balls of radius \\( r \\).

***

The conjecture remains open. In our [preprint](https://arxiv.org/abs/2604.11318), we showed the following separator theorem, which is very close to the conjecture.

***

**Theorem:** For any two given integers \\( h, d \in \mathbb{N} \\), and real \\( \epsilon > 0 \\), the following holds: Every \\( n \\)-vertex graph that excludes \\( K_h \\) as a \\( d \\)-fat minor has a balanced separator that can be covered by \\( O(h^4 \cdot n^{1/2 + \epsilon}) \\) balls of radius \\( O(d / \epsilon) \\).

***

The proof of the above theorem follows the flow-based technique of [Korhonen and Lokshtanov](https://arxiv.org/abs/2308.04795) mentioned above. The key difficulty in the proof is to guarantee that the distance between the branch sets (the connected subgraphs \\( B_v \\) and \\( B_e \\)) is sufficiently far in the input graph. This is precisely the reason why we need the \\( \epsilon \\) in the theorem above.

---

## 3. Open problems

Two open problems:

***

**Open Problem 1:** Prove the AST separator conjecture.

***

**Open Problem 2:** Prove the fat-minor separator conjecture.

***

Regarding the fat-minor separator conjectures, in our [preprint](https://arxiv.org/abs/2604.11318), we formulated a few related conjectures that could lead to its proof, so be sure to check them out.