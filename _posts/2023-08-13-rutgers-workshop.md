---
title:  "DIMACS Workshop on Modern Techniques in Graph Algorithms"
mathjax: true
layout: post
categories: media
---

I attended [the workshop](https://sites.google.com/view/dimacswmtga/home?authuser=0) in mid-June, and it is one of the best that I've ever attended. Thanks to the organizers, Nicole, Zihan, Prantar, and the DIMACS staff, for working on the logistics and booking flights for all the speakers. 

I missed several talks, mostly on the 1st and the last day of the workshop, due to travel constraints. The talks I attended were all excellent. Here I give my brief take on each talk, which is likely erroneous and incomplete. 


The first two talks I attended were given by Rasmus Kyng. Rasmus gave two tutorials on the breakthrough maxflow result. The 1st tutorial was about formulating the framework of the interior point method (IPM) for solving maxflow and min-cost flow: write down an LP, choose an appropriate barrier and potential function, find the gradient, update the current solution, and repeat. The number of iterations is \\(m^{1+o(1)}\\), and the (amortized) time to perform the update in each iteration is \\(m^{o(1)}\\), leading to nearly linear time overall. One key step is to solve the **undirected** min-ratio cycle problem, a combinatorial problem, in each iteration. Thus, IPM can be seen as reducing solving a problem in directed graphs to undirected ones. The second tutorial is about designing a data dynamic structure for solving min-ratio cycle problems quickly in \\(m^{o(1)}\\) amortized time in each iteration. The basic idea is using a low-stretch spanning tree: given a spanning tree that has a low average stretch, there exists a fundamental cycle of the tree that is a good approximate min-ratio cycle. The data structure then has to maintain (a few) low-stretch spanning trees under updates on the edge lengths of the graph. Many interesting ideas go into the data structure; two major ideas are: maintaining a hierarchy of graphs (vertex sparsification) and dynamic spanners (edge sparsification). See [part 1](https://www.youtube.com/watch?v=zxvl0q52tvs) and [part 2](https://www.youtube.com/watch?v=ISsMYFt7z7k) of Ramus talks. 

The talk by Aaron Bernstein is a gentle introduction to *matching sparsifiers*. As the name suggests, a matching sparsifier of a graph is a sparse subgraph that approximately preserves the maximum matching. The talk started off with some possible definitions of a matching sparsifier and analyzed why they are too weak or too strong, despite being very intuitive. The talk suggested that a good definition should be **robust**, **composable**, **easy to work with**, and of course, **having a good approximation guarantee**. Aaron gave several definitions and analyzed their properties. I particularly like the flow of this talk: it progressively built up a good notion of a matching sparsifier. All in all, a great talk. Check out [here](https://www.youtube.com/watch?v=-4of1ZSYbKQ). 

The blackboard talk by Sepehr Assadi on Ruzsa-Szemeredi graphs was a hit. To quote Sepehr: 

> Ruzsa-Szemeredi Graphs (RS graphs henceforth) are a family of extremal graphs with a magical property: they are "locally sparse" and yet "globally dense."

I've seen RS graphs popping up in a [couple](https://arxiv.org/pdf/1605.01106.pdf) [of](https://arxiv.org/pdf/2212.11944.pdf) [algorithm papers](https://arxiv.org/pdf/2207.09354.pdf), but never had a chance to see RS graphs "in action ."The talk of Sepehr Assadi taught me a great deal; see it [here](https://www.youtube.com/watch?v=GCJtZohiBFU).

Sayan Bhattacharya and Soheil Behnezhad gave two different talks on matching in two interrelated settings: dynamic and sublinear, respectively. Matching has been studied for ages, and it is unsurprisingly to see so many different kinds of results on matching. Sayan's talk gave visual guidance to navigate this jungle of results on dynamic matching. To quote Sayan, the big open problem in dynamic matching is:

> Big Open Question: \\((1+\epsilon)\\)-approximation in \\(O(\mathrm{poly}(\log n, 1/\epsilon))\\) update time. 

And it seems we are still very far from having the solution to the problem. Recent progress has made an intimate connection to sublinear algorithms for maximum matching, which is the topic of Soheil's talk.

Soheil surveyed recent developments in estimating the size of the maximum matching in sublinear time in the adjacency list model. Classical algorithms assume the maximum degree of the graph \\(\Delta\\) is small and seek a running time depending only on \\(\Delta\\) and \\(\epsilon\\), the approximation error. But none of these algorithms has a subquadratic running time when \\(\Delta = \Omega(n)\\). Modern algorithms have sublinear time even when  \\(\Delta = \Omega(n)\\). Check out the talk by [Sayan](https://www.youtube.com/watch?v=XL1HNer_uSM) and [Soheil](https://www.youtube.com/watch?v=8TJw_nr83ko).

Hsien-Chih Chang and I talked about planar graphs. My talk focused on the similarity between the geometry of planar graphs and point sets on the Euclidean plane. A well-known fact is that shortest-path metrics of planar graphs do not embed well into the Euclidean plane (and higher dimensional Euclidean spaces.) But this is not the end of the story. In my talk, I described several positive results, illustrating the striking similarity between the geometry of planar graphs and the Euclidean plane: embeddings into small treewidth graphs, VC dimension, and tree cover. These results have been applied to solve various algorithmic problems in planar graphs.

Hsien-Chih gave a talk on our [recent result](https://arxiv.org/abs/2306.06215) joint with others: a tree cover of \\(O(1)\\) trees for planar metrics. The star of the dish is the new concept of shortcut partition. Hsien-Chih gave a brief intuition of the concept, which in some sense, attempts to capture (ir)regularities of planar graphs. We [recently](https://arxiv.org/abs/2308.00555) exploit the shortcut partition to solve the Steiner Point Removal (SPR) problem, as well as several other problems, in minor-free graphs. I hope to write more about this in the future. Check out [my talk](https://www.youtube.com/watch?v=H6X_SoiemMI) and [Hsien-Chih's talk](https://www.youtube.com/watch?v=6EnxGdT2MAU). 

Greg Bodwin's talk is a gem, introducing the notion of bridge girth as a unification of network design. I saw a version of this talk before in the [FOCS 22 metric embedding workshop](https://hackmd.io/@3S70qBUwTR6_CErLY2dm4A/SJfp46KGi) I co-organized with Arnold Filtser, but it still interested me greatly the second time. There are a vast number of network design problems: spanners, distance oracles, emulators, hopsets, matching covers, and Steiner forest, to name a few. And they are all different. But somehow the mysterious bridge girth unifies them all. If you haven't seen this talk, it's time to [check it out](https://www.youtube.com/watch?v=-0vDQpuiYG8).

The last talk I was able to attend was given by Jan van den Brand on solving the multi-commodity flow problem with high accuracy. Here "high accuracy" means that one seeks a \\((1+\epsilon)\\)-approximation with a running time dependency of \\(\mathrm{polylog}(1/\epsilon)\\). Solving the  2-commodity flow problem in *sparse* graphs, exactly or approximately with high accuracy, is equivalent to linear programming. This hardness leaves a possibility of a faster algorithm for multi-commodity flows in *dense* graphs, which is the new result in Jan's talk: an improved algorithm for dense graphs via graph-based techniques. The main bottleneck is the so-called finding *heavy hitters* in the \\(k\\)-commodity incidence matrix. The idea is to reduce the \\(k\\)-commodity incidence matrix to (graph) incidence matrix, then uses expander decomposition to solve the problem, hence the name graph-based techniques.  [Here](https://www.youtube.com/watch?v=66KwZRTc7bI) is the link to the talk.

There are many interesting talks that I missed for no good reason. Fortunately, all the videos were posted [online](https://www.youtube.com/@DIMACS_CCICADA/videos).  




