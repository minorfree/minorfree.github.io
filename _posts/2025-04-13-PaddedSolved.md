---
title:  "Padded Decomposition for Minor-Free Graphs Solved!"
mathjax: true
layout: post
categories: media
---


[Jonathan Conroy](https://jonathan-conroy.github.io/) and [Arnold Filtser](https://arnold.filtser.com/) have recently solved the padded decomposition problem for minor-free graphs. Congratulations to both! I wrote about this open problem [here](https://minorfree.github.io/PaddedTW/) in the past.

A \\((\beta,\Delta)\\)-padded decomposition of \\(G = (V,E,w)\\) is a  *stochastic* decomposition of \\(V\\) into a set \\(\mathcal{P}\\) of clusters of (weak) diameter at most \\(\Delta\\) such that for every \\(v\in V\\), the probability that the ball of radius \\(\gamma\Delta\\) from \\(v\\) is entirely contained in the cluster of \\(v\\), denoted by \\(\mathcal{P(v)}\\), is small. More precisely:

$$\mathrm{Pr}[B_G(v,\gamma \Delta)\subseteq \mathcal{P(v)}] \leq e^{-\beta\gamma} \quad \text{for every }\gamma\ll 1$$ 

A major open problem was: Can we construct a padded decomposition for \\(K_r\\)-minor-fre graphs with a padding parameter \\(\beta = O(\log r)\\)? There has been various partial progress discussed in the [previous post](https://minorfree.github.io/PaddedTW/) for special cases such as bounded genus graphs and bounded treewidth graphs. However for \\(K_r\\)-minor-fre graphs, the best known bound is \\(\beta = O(r)\\) also by [Arnold](https://arxiv.org/abs/1906.09783). As the title suggested, [Jonathan and Arnold ](https://arxiv.org/abs/2504.00278) have recently solved this problem; the paper will appear in STOC 25.


***
**Theorem** [[Conroy and Filtser](https://arxiv.org/abs/2504.00278)]: We can construct a (weak-diameter) padded decomposition for \\(K_r\\)-minor-free graphs with a padding parameter \\(\beta = O(\log r)\\) in polynomial time.

***

I highly recommend readers look at Section 2 of their paper for an excellent summary of existing techniques and new ideas. 

The proof has two major steps: (1) construct a sparse cover with sparsity \\(s =\mathrm{poly}(r)\\) (formal definition given below) and (2) construct a padded decomposition from a sparse cover with padding parameter \\(\beta = O(\log(s)) = O(\log r)\\).   The second step uses [standard techniques](https://arxiv.org/abs/1906.09783) and does not need excluding a fixed minor. The first step is a key new contribution.

A \\((t,s,\Delta)\\)-sparse cover is a collection of clusters \\(\mathcal{C} = \{C_1,C_2,\ldots\}\\) such that: 

- [Small diameter:] every cluster has (weak) diameter at most \\(\Delta\\).
- [Padded:] every ball \\(B(v,\Delta/t)\\) is  wholy contained in some cluster. 
- [Low sparsity:] every vertex is contained in at most \\(s\\) clusters in \\(\mathcal{C}\\). 


***
**Lemma** [[Conroy and Filtser](https://arxiv.org/abs/2504.00278)]: We can construct a  \\((9,s = O(r^4),\Delta)\\)-sparse cover for \\(K_r\\)-minor-free graphs in polynomial time.

***

As mentioned above, the sparse cover implies a padded decomposition with padding parameter \\(\beta = O(\log(s)) = O(\log r)\\).

What I found interesting is that the sparse cover is constructed from the *buffered cop decomposition*. (Indeed, the construction is rather simple.) The buffered cop decomposition was introduced in [our paper](https://arxiv.org/abs/2308.00555) (with Jonathan) for solving the Steiner point removal problem in minor-free graphs. (I [wrote]((https://minorfree.github.io/SPR/)) about this problem in the past.) The precise definition of a buffered cop decomposition is too technical to recall here. Instead, I would add that it has been used in several recent papers: [1](https://arxiv.org/abs/2308.00555), [2](https://arxiv.org/abs/2401.14060), [3](https://arxiv.org/abs/2410.10191), [4](https://arxiv.org/abs/2504.00278). If you are working on a problem in planar and minor-free graphs, be sure to check it out. 

Lastly, I would like to point out that the padded decomposition by Jonathan and Arnold only has a weak diameter: each cluster \\(G[C]\\) might have an unbounded diameter. For many applications, we want \\(G[C]\\) to have a diameter at most \\(\Delta\\); such a padded decomposition is said to have a strong diameter \\(\Delta\\).



***
**Open Problem**: Can one construct a strong-diameter padded decomposition for \\(K_r\\)-minor-free graphs with a padding parameter \\(\beta = O(\log r)\\)? 

***

