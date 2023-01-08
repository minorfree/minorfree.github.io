---
title:  "Shortcutting Trees"
mathjax: true
layout: post
categories: media
---

Over the past two years or so, I have been thinking about a cute problem, which turned out to be much more useful to my research than I initially thought. Here it is:

***
**Tree Shortcutting Problem**: Given an edge-weighted tree \\(T\\), add (weighted) edges to \\(T\\), called *shortcuts*, to get a graph \\(K\\) such that: 
1. \\(d_K(u,v) = d_T(u,v)~\quad \forall u,v\in V(T)\\). That is, \\(K\\) preserves distances in \\(T\\). 
2. For every \\(u,v\in V(T)\\), there exists a shortest path from \\(u\\) to \\(v\\) in \\(K\\) containing at most \\(k\\) edges for some \\(k\geq 2\\).

The goal is to minimize the product \\(k \cdot \mathrm{tw}(K)\\), where \\(\mathrm{tw}(K)\\) is the treewidth of \\(K\\).

***


![](/assets/figs/shortcut.svg)


*Figure 1: An emulator \\(K\\) obtained by adding one edge to the tree \\(T\\) has hop bound \\(3\\) and treewidth \\(2\\). Compared to \\(T\\), the hop bound decreases by 1 while the treewidth increases by 1.*

Such a graph \\(K\\) is called a *low-hop and low-treewidth emulator* of the tree \\(T\\).  The parameter \\(k\\) is called the *hop bound* of \\(K\\). It is expected that there will be some trade-off between the hop bound and the treewidth. We are interested in minimizing \\(k \cdot \mathrm{tw}(K)\\). This product directly affects parameters in our application; see the conclusion section for more details. 

For readers who are not familar with treeewidth, see  [here](https://en.wikipedia.org/wiki/Treewidth) and [here](https://www.win.tue.nl/~nikhil/courses/2015/2WO08/treewidth-erickson.pdf) for an excellent introduction, and why treewidth is an interesting graph parameter.

> **Remark 1:** The tree shortcutting problem is already non-trivial for unweighted trees; the shortcuts must be weighted though. Furthermore, for each edge \\((u,v)\\) added to \\(K\\), the weight of the edge will be \\(d_T(u,v)\\). Thus, in the construction below, we do not explicitly assign weights to the added edges. 

A version of a tree shortcutting problem where one seeks to minimize *the number of edges of \\(K\\)*, given a hop bound \\(k\\), was studied extensively (see [1](http://www.math.tau.ac.il/~haimk/adv-ds-2008/Alon-Schieber.ps),[2](https://dl.acm.org/doi/abs/10.5555/640186.640193),[3](https://arxiv.org/abs/1005.4155),[4](https://dl.acm.org/doi/10.1145/800070.802185), including my own work [with](https://arxiv.org/abs/2107.14221) [others](https://arxiv.org/abs/2112.09124)), arising in the context of spanners and minimum spanning tree problem. What is interesting there IMO is that we see all kinds of crazy slowly growing functions in computer science: for \\(k = 2\\), the number of edges of \\(K\\) is \\(\Theta(n \log n)\\);  for \\(k = 3\\), the number of edges of \\(K\\) is \\(\Theta(n \log\log n)\\); for \\(k = 4\\), the number of edges is \\(\Theta(n \log^* n)\\); \\(\ldots\\) [too difficult to describe]; and for \\(k = \alpha(n)\\), the number of edges is   \\(\Theta(n)\\). Here, as you might guess, \\(\alpha(\cdot)\\) is the notorious (one parameter) inverse Ackermann function. (The \\(\Theta\\) notation in the number of edges means there exist matching lower bounds.) I hope to cover this problem in a future blog post.

Now back to our tree shortcutting problem. Let \\(n = \lvert V(T) \rvert\\). There are two extreme regimes that I am aware of:

1. Hop bound \\(k=2\\) and treewidth \\(\mathrm{tw}(K) = O(\log n)\\). This is a relatively simple exercise. 
2. Hop bound \\(k=O(\log n)\\) and treewidth \\(\mathrm{tw}(K) = O(1)\\). This regime is harder to prove; trying to show this for a path graph will be an insightful exercise. It follows from a well-known fact [1] that any tree decomposition of width \\(t\\) can be turned into a tree decomposition of width \\(O(t)\\) and depth \\(O(\log n)\\). 

The two regimes might suggest that a lower bound \\(k\cdot \mathrm{tw}(K) = \Omega(\log n)\\) for any \\(k\\). In our recent paper [2], we show that this is not the case:

***
**Theorem 1**: There exists an emulator \\(K\\) for any \\(n\\)-vertex tree \\(T\\) such that \\(h(K) = O(\log  \log n)\\) and \\(\mathrm{tw}(K) = O(\log \log n)\\).

***

Theorem 1 implies that one can get an emulator with \\(k\cdot \mathrm{tw}(K) = O((\log \log n)^2)\\), which is exponentially smaller than \\(O(\log(n))\\). The goal of this post is to discuss the proof of Theorem 1. See the conclusion section for a more thorough discussion on other aspects of Theorem 1, in particular, the construction time and application.

For the tree shortcutting problem, it is often insightful to look into the **path graph with \\(n\\) vertices**, which is a special case. Once we solve the path graph, extending the ideas to trees is not that difficult.

# 1. The Path Graph

The (unweighted )path graph \\(P_n\\) is a path of \\(n\\) vertices. To simplify the presentation, assume that \\(\sqrt{n}\\) is an integer. The construction is recursive and described in the pseudo-code below.  First we divide \\(P_n\\) into \\(\sqrt{n}\\) sub paths of size \\(\sqrt{n}\\) each. Denote the endpoints of these subpaths by \\(b_i = i\sqrt{n}\\) for \\(1\leq i\leq\sqrt{n}\\). We call \\(\{b_i\}_{i}\\) boundary vertices.  We have two types of recursions: (1) top level recursion -- lines 2 and 3 -- and (2) subpath recursion -- lines 5 to 9. The intuition of the top level recursion is a bit tricky and we will get to that later. See Figure 2.

![](/assets/figs/recursion.svg)


*Figure 2: (a) The recursive construction applied to the path \\(P_n\\). (b) Gluing \\(\mathcal T_B\\) and all \\(\\{\mathcal T_i\\}\\) to obtain a tree decomposition \\(\mathcal{T}\\) of \\(K\\) via red edges.*

The subpath recursion is natural: recursively shortcut each subpath \\(P[b_i,b_{i+1}]\\) (line 6). The subpath recursion returns the shortcut graph \\(K_i\\) and its tree decomposition \\(\mathcal T_i\\).  Next, we add edges from each boundary vertex \\(b_i, b_{i+1}\\) of the subpath to all other vertices on the subpath (lines 7 and 8). This step guarantees that each vertex of the subpath can "jump" to the boundary vertices using only **one edge**. In terms of tree decomposition, it means that we add both \\(b_i\\) and \\(b_{i+1}\\) to every bag of \\(\mathcal T_i\\) (line 9).

The top level recursion serves two purposes: (i) creating a low hop emulator  for boundary vertices (recall that each vertex can jump to a boundary vertex in the same subpath using one edge) and  (ii) gluing \\(\{\mathcal T_i\}\\) together. More precisely, let \\(P_{\sqrt{n}}\\) be a path of boundary vertices, i.e., \\(b_i\\) is adjacent to \\(b_{i+1}\\) in  \\(P_{\sqrt{n}}\\). We shortcut \\(P_{\sqrt{n}}\\) recursively, getting the shortcut graph \\(K_B\\) and its tree decomposition \\(\mathcal T_B\\) (line 3). Since \\((b_i,b_{i+1})\\) is an edge in \\(P_{\sqrt{n}}\\), there must be a bag in \\(\mathcal T_B\\) containing both \\(b_i,b_{i+1}\\); that is, the bag \\(X\\) in line 11 exists. See Figure 2(b). Recall that in line 9, every bag in \\(\mathcal T_i\\) contains both \\(b_i,b_{i+1}\\), and that \\(K_B\\) and \\(K_i\\) only share two boundary vertices \\(b_i,b_{i+1}\\). Thus, we can connect \\(X\\) to an arbitrary bag of \\(\mathcal T_i\\) as done in line 12. This completes the shortcutting algorithm.

***
 <span style="font-variant: small-caps">PathShortcutting</span>\\((P_n)\\)
> \\(1.\\) \\(B \leftarrow \{0,\sqrt{n}, 2\sqrt{n}, \ldots, n\}\\) and \\(b_i \leftarrow i\sqrt{n}\\) for every \\(0\leq i \leq \sqrt{n}\\)<br>
> \\(2.\\) \\(P_{\sqrt{n}} \leftarrow\\) unweighted path graph with vertex set \\(B\\).<br>
> \\(3.\\) \\((K_B,\mathcal T_B) \leftarrow\\)<span style="font-variant: small-caps">PathShortcutting</span>\\((P_{\sqrt{n}})\\)<br>
> \\(4.\\) \\(K\leftarrow K_B,\quad \mathcal{T}\leftarrow \mathcal T_B\\)<br>
> \\(5.\\) for \\(i\leftarrow 0\\) to \\(\sqrt{n}-1\\)<br>
> \\(6.\\) &nbsp;&nbsp;&nbsp;&nbsp;\\((K_i,\mathcal T_i) \leftarrow\\)<span style="font-variant: small-caps">PathShortcutting</span>\\((P_{n}[b_i, b_{i+1}])\\)<br>
> \\(7.\\) &nbsp;&nbsp;&nbsp;&nbsp; for each \\(v\in P_{n}[b_i, b_{i+1}]\\)<br>
> \\(8.\\) &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; \\(E(K_i)\leftarrow \{(v,b_i), (v,b_{i+1})\}\\) <br>
> \\(9.\\) &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;  add both \\(\{v_i,v_{i+1}\}\\) to every bag of \\(\mathcal T_i\\)<br>
> \\(10.\\) &nbsp;&nbsp;&nbsp; \\(K\leftarrow K \cup K_i\\) <br>
> \\(11.\\) &nbsp;&nbsp;&nbsp; Let \\(X\\) be a bag in \\(\mathcal{T}\\) containing both \\(b_i,b_{i+1}\\)<br>
> \\(12.\\) &nbsp;&nbsp;&nbsp; Add \\(\mathcal T_i\\) to \\(\mathcal{T}\\) by connecting \\(X\\) to an arbitrary bag of \\(\mathcal T_i\\) <br>
> \\(13.\\) return \\((K,\mathcal{T})\\)

***


It is not difficult to show that \\(\mathcal{T}\\) indeed is a tree decomposition of \\(K\\). Thus, we focus on analyzing the hop bound and the treewidth.

> **Remark 2:** For notational convenience, we include \\(0\\) in the set \\(B\\) though \\(0\not\in P_n\\). When calling the recursion, one could simply drop 0.

![](/assets/figs/hopbound.svg)


*Figure 3: A low-hop path from \\(u\\) to \\(v\\).*

**Analyzing the hop bound \\(h(K)\\).**   Let \\(u\\) and \\(v\\) be any two vertices of \\(P_{n}\\); w.l.o.g, assume that \\(u \leq v\\), and \\(h(n)\\) be the hop bound. Let \\(b_{u}\\) and \\(b_v\\) be two boundary vertices of the subpaths containing \\(u\\) and \\(v\\), respectively, such that \\(b_u,b_v \in P[u,v]\\). See Figure 3. As mentioned above, line 8 of the algorithms guarantees that there are two edges \\((u,b_u)\\) and \\((b_v,v)\\) in \\(K\\), and the top level recursion (line 3) guarantees that there is a shortest path of hop length  \\(h(\sqrt{n})\\) between \\(b_u\\) and \\(b_v\\) in \\(K_B\\). Thus we have:

$$ h(n) \leq h(\sqrt{n}) + 2 $$

which solves to \\(h(n)= O(\log\log n)\\).


**Analyzing the treewidth \\(\mathrm{tw}(K)\\).** Note that the treewidth of   \\(\mathcal T_B\\) and all \\(\{\mathcal{T_i}\}\\) (before adding boundary vertices in line 9) is bounded by \\(\mathrm{tw}(\sqrt{n})\\). Line 9 increases the treewidth of  \\(\{\mathcal{T_i}\}\\) by at most \\(2\\). Since the treewidth of \\(K\\) is the maximum treewidth \\(\mathcal T_B\\) and all \\(\{\mathcal{T_i}\}\\), we have:

$$ \mathrm{tw}(n) \leq \mathrm{tw}(\sqrt{n}) + 2 $$

which solves to \\(\mathrm{tw}(n)= O(\log\log n)\\).

This completes the proof of Theorem 1 for the path graph \\(P_n\\).

# 2. Trees

What is needed to extend the construction of a path graph to a general tree? Two properties we exploited in the construction of \\(P_n\\):

1. \\(P_n\\) can be decomposed into \\(\sqrt{n}\\) (connected) subpaths.
2. Each subpath in the decomposition has at most two boundary vertices.

Property 2 implies that the total number of boundary vertices is about \\(\sqrt{n}\\), which plays a key role in analyzing the top level recursion.

It is well known that we can obtain a somewhat similar but weaker decomposition for trees: one can decompose a tree of \\(n\\) vertices to roughly \\(\sqrt{n}\\) connected subtrees such that the number of boundary vertices is \\(\sqrt{n}\\). (A vertex is a boundary vertex of a subtree if it is incident to an edge not in the subtree.) This decomposition is weaker in the sense that a subtree could have more than 2, and indeed up to \\(\Omega(\sqrt{n})\\), boundary vertices.  Is this enough?

Not quite. To glue the tree decomposition \\(\mathcal T_i\\) to \\(\mathcal{T}\\) (and effectively to \\(\mathcal T_B\\)), we rely on the fact that there is a bag \\(X\in \mathcal T_B\\) containing both boundary vertices in line 11. The  analogy for trees would be: there exists a bag \\(X\\) containing all boundary vertices of each subtree. This is problematic if a subtree has \\(\Omega(\sqrt{n})\\) boundary vertices. 

Then how abound guaranteeing that each subtree has \\(O(1)\\) vertices, say 3 vertices. Will this be enough? The answer is pathetically no. To guarantee 3 vertices in the same bag, one has to add a clique of size 3 between the boundary vertices in the top level recursion. What it means is that, the graph between boundary vertices on which we recursively call the shortcuting procedure is *no longer a tree*. Thus, we really need a decomposition where every subtree has **at most 2 boundary vertices**.  


***
**Lemma 1:** Let \\(T\\) be any tree of \\(n\\) vertices, one can decompose \\(T\\) into a collection \\(\mathcal{D}\\) of \\(O(\sqrt{n})\\) subtrees such that every tree \\(T'\in \mathcal{D}\\) has \\(\lvert V(T')\rvert \leq \sqrt{n}\\) and at most 2 boundary vertices.

***

Lemma 1 is all we need to prove Theorem 1, following exactly the same construction for the path graph \\(P_n\\); the details are left to readers. 

>  **Remark 3:** In developing our shortcutting tree result, we were unaware that Lemma 1 was already known in the literature. A reviewer later pointed out that one can get Lemma 1 from a weaker decomposition using *least common ancestor closure* [3], which I reproduce below.

**Proof of Lemma 1:** First, decompose \\(T\\) into a collection \\(\mathcal{D}'\\) of \\(O(\sqrt{n})\\) subtrees such that each tree in \\(\mathcal{D}'\\) has size at most \\(\sqrt{n}\\) and that the total number of boundary vertices is \\(O(\sqrt{n})\\). As mentioned above, this decomposition is well known; see Claim 1 in our paper [2] for a proof.  

Let \\(A_1\\) be the set of boundary vertices; \\(\lvert A_1\rvert = O(\sqrt{n})\\). Root \\(T\\) at an arbitrary vertex. Let \\(A_2\\) be the set containing the ancestor of every pair of vertices in \\(A_1\\). Let \\(B = A_1\cup A_2\\).

It is not hard to see that \\(\lvert A_2\rvert  \leq \lvert A_1\rvert - 1  = O(\sqrt{n})\\). Thus, \\(\lvert B\rvert = O(\sqrt{n})\\). Furthermore, every connected component of \\(T\setminus B\\) has at most \\(\sqrt{n}\\) vertices and has edges to at most 2 vertices in \\(B\\). The set \\(B\\) induces a decomposition \\(\mathcal{D}\\) of \\(T\\) claimed in Lemma 1.
 

# 3. Conclusion

The emulator in Theorem 1 can be constructed in time \\(O(n \log \log n)\\); see Theorem 10 our paper [2] for more details. The major open problem is:

**Open problem:** Is \\(O((\log \log n)^2)\\) the best possible bound for the product \\(\mathrm{tw}(K)\cdot h(K)\\)?

This open problem is intimately connected to another problem: embedding  planar graphs of diameter \\(D\\) into low treewidth graphs with additive distortion at most \\(+\epsilon D\\) for any \\(\epsilon \in (0,1)\\). More precisely, though not explicitly stated [2], one of our main results is:


***
**Theorem 2:** If one can construct an emulator \\(K\\) of treewidth \\(\mathrm{tw}(n)\\) and hop bound \\(h(n)\\) for any tree of \\(n\\) vertices, then one can embed any planar graphs with \\(n\\) vertices and diameter \\(D\\) into  a graph of treewidth \\(O(h(n)\cdot \mathrm{tw}(n)/\epsilon)\\) and additive distortion \\(+\epsilon D\\) for any given \\(\epsilon \in (0,1)\\). 

***

That is, the product of treewdith and hop bound directly bounds the treewdith of the embedding. Theorem 1 give us an embedding with treewidth \\(O((\log\log n)^2/\epsilon)\\), which has various algorithmic applications [2]. My belief is that the bound \\(O((\log \log n)^2)\\) is the best possible.

# 4. References

[1] Bodlaender, H.L. and Hagerup, T. (1995). Parallel algorithms with optimal speedup for bounded treewidth. In ICALP '95, 268-279.

[2] Filtser, A. and Hung, L. (2022). Low Treewidth Embeddings of Planar and Minor-Free Metrics. ArXiv preprint [arXiv:2203.15627](https://arxiv.org/abs/2203.15627).

[3] Fomin, F.V., Lokshtanov, D., Saurabh, S. and Zehavi, M. (2019). Kernelization: theory of parameterized preprocessing. Cambridge University Press.
