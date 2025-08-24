---
title:  "Some Questions on Spanners"
mathjax: true
layout: post
categories: media
---

I gave a talk at the [Sublinear Graph Simplification workshop](https://simons.berkeley.edu/workshops/sublinear-graph-simplification) at the Simons Institute for Theory of Computing in July 2024. My talk titled ["Recent Progress on Euclidean Spanners"](https://www.youtube.com/watch?v=H3KWeEEVA9g), which was my original motivation. However, to fit my talk more to the theme of the workshop, I decided to  expand the scope to cover many different types and classes of graphs. Here is a [copy of my slides](/assets/figs/spanner-simons-talk.pdf).

In the talk, I posed various questions, and since then, there has been progress on some of them. So I think I should post about these questions and include recent progress on each of them. I wish to see more progress in this space. Without further ado, here they are (refer to my slides for terminology).

---

**Question 1**: Show the lightness lower bound \\(\Omega(n^{1/k}/\epsilon)\\) for \\((2k-1)(1+\epsilon)\\)-spanners of general graphs (even assuming Erdős’ Girth Conjecture).


> Comments: [Bodwin and Flics](https://arxiv.org/pdf/2406.04459) showed a lower bound of \\(\Omega(n^{1/k}/\epsilon^{1/k})\\) assuming Erdős’ Girth Conjecture. They also discussed the difficulty of extending the lower bound further.

> Status: Open.

---


**Question 2**: Construct light \\(f\\)-fault-tolerant (vertex/edge) graph spanners?

> Comments: This question is vague since one has to be careful with the notion of lightness. Then this [beautiful paper](https://arxiv.org/pdf/2502.10890) by Bodwin, Dinitz, Koranteng, and Wang came out, clarifying all subtle issues. Natural attempt to define lightness fails, and they proposed a very interesting notion of lightness. 

> Status: Partially solved.

---


**Question 3**: Light bounded degree fault-tolerant spanners in Euclidean and doubling metrics with lightness \\(O_{\epsilon,d}(f)\\)? 


> Comments: The bounded degree faulure model was introduced by [Bodwin, Haeupler, and  Parter](https://arxiv.org/abs/2309.06696). [This paper](https://arxiv.org/abs/2405.18134) then constructed a bounded degree fault-tolerant spanner for Euclidean and doubling metrics with sparsity \\(O_{\epsilon,d}(f)\\) (in Euclidean) and \\(O_{\epsilon,d}(f^2)\\)? for doubling. There is a question of closing this gap (mentioned in my talk). However, constructing a light version seems more challenging. 

> Status: Open.

---

**Question 4**: Other models of massive failure? 


> Comments:A model for massive failure should allow up to \\(\Omega(n)\\) nodes or edges to fail, while non-trivial guarantees (sparsity/lightness) are still achievable. In the talk, I mentioned four models: [reliable spanners](https://arxiv.org/pdf/2007.08738), [dependable spanners](https://arxiv.org/abs/2407.01466), [color fault-tolerant spanners](https://arxiv.org/abs/2311.08868)), and [bounded-degree spanners](https://arxiv.org/abs/2309.06696). Any other reasonable model?

> Status: Open.

---

**Question 5**: \\(n^{\epsilon}\\)-approximation for \\(t\\)-spanners for a constant \\(t\\)? 


> Comments: Approximating spanners is a basic but extremely diffficult problem. Even for very small \\(t  = 4\\), the best approximation ratio is \\(\tilde{O}(n^{1/4})\\). A polylog-approximation was ruled out, but \\(n^{\epsilon}\\)-approximation for any constant \\(\epsilon\\) is still possible.

> Status: Open.

---

**Question 6**: \\(O(1)\\)-approximation for \\((1+\epsilon)\\)-spanners for point set in \\(\mathbb{R}^2\\). 

> Comments: There is a long line of work on Euclidean spanners. However, approximating spanners in this setting is very poorly understood. My [recent work](https://arxiv.org/abs/2409.08227) with Shay, Cuong, Csaba, and Tianyi only gave a bi-criteria approximation algorithm (approximating both the stretch and the number of edges/total edge weights). The above question asks for a single-criterion approximation in the simplest possible setting. Not that a PTAS might well be possible, but it seems extremely difficult.

> Status: Open.

----

**Question 7**: \\(O(\log(n))\\)-approximation for \\(t\\)-spanners for point set in high dimensional \\(\mathbb{R}^d\\) (where \\(d = \Omega(\log n)\\)). 

> Comments: The best approximation ratio is the same as for general graphs. As mentioned above, a \\(polylog(n)\\)-approximation was ruled out for general graphs, but no such lower bound is known for Euclidean graphs. 

> Status: Open.

----

**Question 8**: (Bicriteria-)approximation algorithm for spanners in doubling metrics. 

> Comments: Results for Euclidean spanners often translate naturally to doubling metrics. However, for approximation algorithms, the story is much more complicated. Even our bicriteria-approximation algorithm for Euclidean spanners in the comments of Question 6 does not translate to doubling metrics. Could we achieve any non-trivial approximation for spanners in doubling metrics?

> Status: Open.

----

**Question 10**: What is the smallest stretch achieved by a tree cover with only two trees for general graphs?

> Comments: A \\(\tilde{O}(\sqrt{n})\\) stretch is known by [Bartal, Fandina, and Neiman](https://arxiv.org/abs/1905.07559). Indeed, their tree cover is Ramsey. My question is more about proving a non-trivial lower bound. 

> Status: Solved. The answer is \\(\tilde{\Theta}(\sqrt{n})\\) See [this recent beautiful work](https://arxiv.org/abs/2508.10376) by Chen, Tan, and Xu.

----

**Question 11**: Other structural alternatives to spanners?

> Comments: This question is intentionally vague. I view tree covers and locality-sensitive orderings as two structural versions of spanners, where the basic objects are trees and paths, respectively. As trees and paths are very simple, both tree covers and locality-sensitive orderings have many algorithmic applications. Therefore, I would love to see more "structural versions" of spanners.

> Status: Open.