---
title:  "Optimal Euclidean Tree Covers"
mathjax: true
layout: post
categories: media
---
# Optimal Euclidean Tree Covers

This post will discuss [our recent preprint](https://arxiv.org/abs/2403.17754) on tree cover for Euclidean metrics.  A line of research that I have been thinking a lot about recently is constructing tree covers for point sets in metric spaces. Intuitively, a tree cover of a point set is a collection of trees such that for any two points in the set, one of the trees preserves their distance up to some stretch factor. More formally, given a set of point \\(X\\) in a metric space \\((M,\delta_M)\\), a *tree cover of stretch \\(\alpha\\)* for \\(P\\) is a collection of trees \\({\mathcal T}\\) such that: 
1. [Dominating] for every tree \\(T\in {\mathcal T}\\), \\(X\subseteq V(T)\\), and for every two points \\(x,y\in X\\), \\(\delta_M(x,y)\leq \delta_T(x,y)\\).
2. [Stretch] for every tow points \\(x,y\in X\\), there exists a tree \\(T\in{\mathcal T}\\) such that \\(\delta_T(x,y)\leq \alpha\cdot \delta_M(x,y)\\).

The study of tree cover for general graph metrics dates back to 1992, starting from the paper by [Awerbuch and Peleg](https://epubs.siam.org/doi/10.1137/0405013) on communication-space trade-off in routing; refer to a short survey on tree conver in the introduction of [our paper](https://arxiv.org/abs/2403.17754) for more detail. 

Tree covers have many algorithmic applications, but my favorite is in constructing an approximate distance oracle, which got me to think more about tree covers in the first place. Say we want to construct an approximate distance oracle for a graph \\(G\\) with a stretch factor (i.e., distance error) of \\(t\\), we first construct a tree cover with stretch \\(t\\) for (the shortest path metric of) \\(G\\). Then, we construct an exact distance oracle for each tree, that can be reduced to [LCA data structure](https://en.wikipedia.org/wiki/Lowest_common_ancestor).  To answer a distance query between two vertices \\(x\\) and \\(y\\), all we have to do is go through each tree in the cover, query the \\(x\\)-to-\\(y\\) distance in the tree, and then return the minimum distance among all the trees.  The number of trees in the cover will dictate the space of the oracle and the query time; more precisely, \\(O(n \lvert{\mathcal T}\rvert)\\) space and \\(O(\lvert{\mathcal T}\rvert)\\) query time. The distance approximation factor is the stretch of the tree cover.

As we have seen, in distance oracles as well as other applications of tree cover, the stretch and the number of trees will be important parameters, and determining the precise trade-off between the two parameters is the most fundamental question. 

I am personally interested in constructing tree covers for special metric spaces, such as [Euclidean](https://arxiv.org/abs/2403.17754), [planar](https://arxiv.org/abs/2306.06215) and [minor-free](https://arxiv.org/abs/2308.00555), and [doubling](https://arxiv.org/abs/2107.14221). In these metrics, the stretch can be made \\((1+\epsilon)\\) for any \\(\epsilon \in (0,1)\\), which makes tree covers attractive for various applications. The main question is: Can the number of trees be independent of \\(n\\), the number of points? It turned out that in all these settings, the number of trees can be made independent of \\(n\\). (This fact was known for Euclidean metrics a long time ago but only recently known for [doulbing](https://arxiv.org/abs/1905.07559), [planar](https://arxiv.org/abs/2306.06215) and [planar](https://arxiv.org/abs/2306.06215) metrics. ) 

Then the next question is: What is the precise dependency on \\(\epsilon\\) of the number of trees? In the [recent preprint](https://arxiv.org/abs/2403.17754), we answered this question.


***
**Theorem**: The number of trees is \\(\Theta(\epsilon^{1-d})\\) for non-Steiner tree covers and \\(\Theta(\epsilon^{(1-d)/2})\\) for Steiner tree covers for point sets in \\(\mathrm{R}^d\\). 

***


In \\(\mathrm{R}^2\\), the number of non-Steinter trees is \\(O(1/\epsilon)\\) while the number of Steiner trees is \\(O(1/\sqrt{\epsilon})\\). But what is the difference between non-Steiner and Steiner?

Recall in the definition (of dominating property) that we only require \\(X\subseteq V(T)\\). If \\(X = V(T)\\) for every tree \\(T\\), then we get a non-Steiner tree cover, and if we allow \\(V(T)\setminus X \not=\emptyset\\), we get a Steiner tree cover since \\(T\\) contains (Steiner) points that are not in \\(X\\). Things get a bit complicated when we consider Steiner points in \\(V(T)\setminus X\\): they could be anything. But here and in our paper, we restrict Steiner points to those in  the ambient metric space \\((M,\delta_M)\\). 

In the rest of the post, I will say a few words about the technique. 

**For the lower bound** of \\(\Omega(\epsilon^{1-d})\\) trees for non-Steiner tree covers, the observation is that by taking the union of all the trees in the cover, we obtain a \\((1+\epsilon)\\)-spanner with \\(\lvert{\mathcal T}\rvert n\\) edges. On the other hand, there is a [known lower bound](https://epubs.siam.org/doi/10.1137/20M1317906) of \\(\Omega(\epsilon^{1-d})n\\) on the number of edges of  \\((1+\epsilon)\\)-spanners. Therefore, one gets \\(\lvert{\mathcal T}\rvert = \Omega(\epsilon^{1-d})\\). The same argument applies to get the lower bound for Steiner tree covers, but this time, we use known lower bounds for [Steiner](https://epubs.siam.org/doi/10.1137/20M1317906) \\((1+\epsilon)\\)-[spanners](https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.STACS.2021.13).

**For the upper bound**, the essential idea is to reduce to the construction of a tree cover for far points:  given a point set \\(P\\) with diameter \\(D\\) and a constant \\(\mu\\), construct a tree cover that only needs to preserve (up to \\(1+\epsilon\\) factor) pairs whose distances are in \\([D/\mu, D]\\). (We call this type of tree cover a *partial tree cover*). Our reduction is based on the shifting technique in [a beautiful paper by Chan](https://graphics.stanford.edu/courses/cs468-06-fall/Papers/05%20chan%20-%20DCG98.pdf).

Once the reduction is in place, we only need to construct a partial tree cover. If we are allowed to use Steiner points, then the construction is relatively simple: we basically divide the bounding box of \\(P\\) into vertical and horizontal slabs of width \\(\Theta(D)\\), and then for any two pairs of slabs, place Steiner points in the middle. The construction of non-Steiner tree covers is significantly more involved; the introduction of [our paper](https://arxiv.org/abs/2403.17754) has a high-level overview of the proof. We also obtain a substantially stronger result than what is stated in the theorem above: every tree in our non-Steiner tree cover has a degree \\(O(1)\\), an absolute constant that does not depend on \\(d\\) and \\(\epsilon\\). 








