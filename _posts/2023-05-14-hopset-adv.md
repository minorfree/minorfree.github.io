---
title:  "Amazing Recent Advances on Shotcut Set and The Likes"
mathjax: true
layout: post
categories: media
---

[Merav Parter](http://www.weizmann.ac.il/math/parter/home) gave a talk at UMass theory seminar last week titled "Reachability Shortcuts: New Bounds and Algorithms". In the talk, Merav summarized recent developments in shortcut sets and related concepts. It was amazing to see all the results, and I cannot resist the attempt to share my take here in this blog post.

# 1. Shortcut set: what is it?

A \\(d\\)-shortcut set of a *directed graph* \\(G=(V,E)\\) is a set of directed edges \\(E_H\\) such that (i) the graph \\(H = (V,E\cup E_H)\\) has the same transitive closure as \\(G\\) and (ii) for every \\(u\not=v\in V\\), if \\(v\\) is reachable from \\(u\\), then there is directed \\(u\\)-to-\\(v\\) path of at most \\(d\\) edges (a.k.a. hops).  

![](/assets/figs/shortcut-set.svg)

*Figure:  Adding two shortcuts (the red dashed edges) reduces the hop distance from \\(u\\) to \\(v\\) to 4. For a shortcut set of linear size, DAG is the most difficult instance: one can shortcut any strongly connected graph to diameter \\(2\\) by \\(n-1\\) edges.*

The shortcut set problem was introduced by [Thorup](https://link.springer.com/chapter/10.1007/3-540-56402-0_48). The main goal is to understand the trade-off between the hop diameter \\(d\\) and the size of the shortcut set \\(E_H\\). The most well-studied regime is fixing \\(\lvert E_H\rvert = \tilde{O}(n)\\), and then minimizing \\(d\\). (Here, \\(n\\) is the number of vertices.) We will call this special regime *the shortcut set problem*. 



For the shortcut set problem, a folklore sampling gives \\(d = O(\sqrt{n})\\): sample each vertex with probability \\(\tilde{O}(1/\sqrt{n})\\) to get a set \\(S\\), and add all (permissible) directed edges between vertices in \\(S\\). The key insight for bounding the diameter is: For any shortest path \\(\pi_G(u,v)\\) from \\(u\\) to \\(v\\) with at least \\(2\sqrt{n}\\) edges,  \\(S\\) will likely hit both the prefix and suffix of length \\(\sqrt{n}\\) of \\(\pi_G(u,v)\\). (This algorithm is attributed to [Ullman and Yannakakis](https://dl.acm.org/doi/10.1145/97444.97686).) A long-standing open problem is:

***
**Question 1**: Is diameter bound \\(\sqrt{n}\\) optimal for shortcut sets of nearly linear size?

***

The answer is no due to the [recent breakthrough](https://arxiv.org/pdf/2111.13240.pdf) by Kogan and Parter: using  \\(\tilde{O}(n)\\) shorcuts, they reduced the diameter down to \\(n^{1/3}\\). Their result deservingly won the best paper award at SODA 22. A key conceptual contribution of their work is a shift in perspective: shortcutting by connecting vertices to paths of length roughly \\(n^{1/3}\\), instead of only making vertex-to-vertex connections as the folklore sampling. Find this intriguing? Read [the paper](https://arxiv.org/pdf/2111.13240.pdf) or watch [Merav's talk](https://www.youtube.com/watch?v=qaY14SnLdMM) or both. The paper has a bunch of other interesting results.

At this point, you might wonder: how far could one go about reducing the diameter? Thorup [conjectured](https://link.springer.com/chapter/10.1007/3-540-56402-0_48) that the diameter could be reduced all the way down to \\(\mathrm{poly}(\log(n))\\) with a nearly linear number of shortcuts. This conjecture was disproved by [Hesse](https://dl.acm.org/doi/pdf/10.5555/644108.644216), who was a graduate student at UMass Amherst at the time with our [Neil Immerman](https://www.mathgenealogy.org/id.php?id=30442) :). Hesse constructed a directed graph with \\(m = \Theta(n^{19/17})\\) edges such that one has to use \\(\Omega(mn^{1/17})\\) shortcuts to reduce the diameter to below \\(\Theta(n^{1/17})\\). This lower bound [has been](https://arxiv.org/abs/1802.06271) [improved](https://arxiv.org/abs/2110.15809) [further](https://arxiv.org/pdf/2304.02193.pdf). Notably, [Bodwin and Hoppenworth](https://arxiv.org/pdf/2304.02193.pdf) recently obtained a lower bound of \\(\Omega(n^{1/4})\\) on the diameter, which is very close to the upper bound of \\(O(n^{1/3})\\) by Kogan and Parter. This result leaves another fascinating open problem:

***
**Open Problem**: Closing the gap \\(O(n^{1/3})\\) vs. \\(\Omega(n^{1/4})\\) on the diameter for shortcut sets of nearly linear size.  

***


# 2. (Approximate) hopsets

Hopset is very similar to the shortcut set but applied to weighted graphs instead. More formally, a \\(d\\)-hopset set of a *directed, weighted graph*  \\(G=(V,E)\\) is a set of directed, *weighted* edges \\(E_H\\) such that (i) the graph \\(H = (V,E\cup E_H)\\) has the same distance metric as \\(G\\) and (ii) for every \\(u\not=v\in V\\),  there is a shortest \\(u\\)-to-\\(v\\) path of at most \\(d\\) edges in \\(H = (V,E\cup E_H)\\). 

Clearly, the hopset problem is at least as hard as the shortcut set problem: any hopset is a shortcut set with the same diameter bound. 

A related notion is *approximate hopset*, in which we are given an additional parameter \\(\epsilon \in (0,1)\\), and for every \\(u,v\\), we would like to have a \\((1+\epsilon)\\)-approximate \\(u\\)-to-\\(v\\) path of at most \\(d\\) edges. We allow \\(d\\) to depend on \\(\epsilon\\)---think of \\(\epsilon\\) as a small constant. 


One could check that the folklore sampling also works for hopset (and hence for approximate hopset as well): by adding \\(\tilde{O}(n \log n)\\) edges, one could reduce the diameter bound to \\(\sqrt{n}\\). The same question arises: Is the  \\(\sqrt{n}\\) bound on the diameter optimal for hopsets and approximate hopsets of nearly linear size? 

For the approximate hopset, the same paper by Kogan and Parter provided a no answer: They constructed a nearly linear hopset with a diameter bound of \\(n^{2/5}\\). In follow-up work, [Bernstein and Wein](https://arxiv.org/abs/2207.04507) improved the diameter to \\(n^{1/3}\\), matching the known bound for shortcut set; [watch the talk here](https://video.cs.utexas.edu/node/428).  

How about the (exact) hopset? One might expect that the diameter bound should also be improved to  \\(n^{1/3}\\). [Bodwin and Hoppenworth](https://arxiv.org/pdf/2304.02193.pdf) recently showed that \\(\sqrt{n}\\) is the best one could do (up to some polylog). I personally find this result very surprising. How do they achieve their lower bound? Read their paper; the exposition of the ideas in the paper is so well written that any attempt to summarize better would be in vain.

Somehow, the exact hopset is strictly harder than the approximate hopset and the shortcut set. Could approximate hopset be strictly harder than shortcut set? Bernstein and Wein provided state-of-the-art bounds for approximate hopsets, matching those of shortcut sets. As far as I understand, their proof does not provide a black-box reduction. Is such a reduction possible? I am curious to know the answer.
