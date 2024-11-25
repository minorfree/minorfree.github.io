---
title:  "Additive Spanners"
mathjax: true
layout: post
categories: media
---

I wrote about [multiplicative spanners](https://minorfree.github.io/hz-spanner/) in the past on this blog. The word "multiplicative" refers to the stretch being multiplicative. This post is devoted to additive spanners, and (a small section) on a [recent work](https://arxiv.org/pdf/2408.14638) with my PhD student [An La](https://anla-cs.github.io/).


***
**Additive \\(k\\)-Spanner**: Given an unweighted graph \\(G\\), a \\(+k\\)-spanner is a subgraph of \\(G\\), denoted by \\(H\\),  such that for every two vertices \\(u,v\in V(G)\\):
$$d_H(u,v)\leq d_G(u,v) + k$$

***

The parameter \\(k\\) is called the additive stretch of \\(H\\). There is a very long line of work on additive spanners, starting from [the work](https://epubs.siam.org/doi/abs/10.1137/S0097539796303421) of Aingworth, Chekuri, Indyk, and Motwani whose result could be interpreted as constructing a \\(+2\\)-spanner with \\(\tilde{O}(n^{3/2})\\) edges. 


![](/assets/figs/path-2.svg)

*Figure 1: (a) A +2-stretch path. (b) A +6 stretch path.*

# A +2-Spanner

The algorithm, due to [Aingworth, Chekuri, Indyk, and Motwani](https://epubs.siam.org/doi/abs/10.1137/S0097539796303421), is very simple and could easily be taught:

1. Add to the spanner, called \\(H\\), every edge incident to a vertex of degree at most \\(\sqrt{n}\\) (called a **low-degree** vertex).

2. Sample every vertex independently with probability \\(p = \frac{10\log n}{\sqrt{n}}\\). Let \\(S\\) be the sampled set. Then build a BFS tree from every vertex in \\(S\\) and add the edges of all the BFS trees to \\(H\\).

It is not hard to see that \\(S\\) has expected size \\(O(\sqrt{n}\log n)\\) and hence the total number of edges added in step 2 (and therefore in both steps) is  \\(O(n^{3/2}\log n)\\). Showing the additive stretch 2 (with high probability) is a bit tricker. 


Consider a shortest path \\(P\\) from a vertex \\(u\\) to a vertex \\(v\\).  If every vertex on \\(P\\) is low-degree, then every edge of \\(P\\) is in \\(H\\) by step 1, and hence the additive stretch is \\(0\\). Otherwise, at least one vertex, say \\(y\\), on \\(P\\) has a high degree, i.e., its degree is at least \\(\sqrt{n}\\). Then by the standard union  bound, it is not hard to show that \\(y\\) has a neighbor \\(x \in S\\) (see Claim 1). See Figure 1(a). By step 2, \\(d_H(x,u) = d_G(x,u)\\) and  \\(d_H(x,v) = d_G(x,v)\\). On the other hand, by the triangle inequality:

$$d_G(x,u) \leq d_G(x,y) + d_G(y,u) = d_G(y,u) + 1$$

$$d_G(x,v) \leq d_G(x,y) + d_G(y,v) = d_G(y,v) + 1$$

and therefore:

$$ d_H(u,v) \leq d_H(x,u) + d_H(x,v) = d_G(x,u) + d_G(x,v \leq  d_G(u,v) + 2$$

as claimed.

***
**Claim 1**: With probability at least  \\(1-1/n^{9}\\), every every vertex of degree at least \\(\sqrt{n}\\) has at least one neighbor in \\(H\\). 

***

**Proof:**  Observe that for every vertex of degree at least \\(\sqrt{n}\\), the probability that none of it neighbors is hit by \\(S\\) is at most \\((1- \frac{10\log n}{\sqrt{n}})^{\sqrt{n}} \approx 1/n^{10}\\). Therefore, by union bound, every high-degree vertex has at least one neighbor in \\(S\\) with probability at least \\(1-n/n^{10} = 1- 1/n^9\\).

***


**Remark 1:** One can construct a \\(+2\\)-spanner with \\(O(n^{3/2})\\) edges deteministically by [this](https://theory.stanford.edu/~virgi/cs267/papers/dorhalperinzwick.pdf) or [this](https://arxiv.org/abs/1403.0178).

# A +6-Spanner 

There are several known algorithms for constructing a +6-spanner with \\(\tilde{O}(n^{4/3})\\) edges. My favorite one is the randomized algorithm by [Woodruff](https://www.cs.cmu.edu/afs/cs/user/dwoodruf/www/w10.pdf). The key idea is to sample vertices according to the number of missing edges which I find very elegant. 

Let \\(P_G(u,v)\\) be a shortest path from \\(u\\) to \\(v\\) in \\(G\\). First, we describe the high-level ideas of the algorithm which is structured differently from the pseudo-code below. 

1. For every vertex \\(u\\), add to the spanner \\(H\\) up to  \\(n^{1/3}\\) incident edges (chosen arbitrarily). This means if \\(u\\) has a degree at most \\(n^{1/3}\\), called a **low-degree** vertex, then all of the incident edges will be added. 

2. Sample every vertex with probability \\(p = \frac{10\log n}{n^{1/3}}\\) to get a set \\(S\\) of sampled vertices; \\(\lvert S\rvert\\) is about \\(\tilde{O}(n^{2/3})\\). For the same reason in Claim 1, \\(S\\) will hit the neighborhood of every high-degree vertex. However, we cannot add a BFS tree from every vertex of \\(S\\), since the total number of edges would be \\(\tilde{O}(n^{5/3})\\), more than our desired \\(\tilde{O}(n^{4/3})\\) bound.

3. Consider the shortest path \\(P_G(u,v)\\) between \\(u\\) and \\(v\\). We can assume that \\(u\\) and \\(v\\) are high-degree vertices; otherwise, we will consider the subpath of \\(P_G(u,v)\\)  between the two high-degree vertices closest to \\(u\\) and to \\(v\\) (the rest of the path is in \\(H\\)).  By union bound, there exists \\(x,y \in S\\) that are neighbors **in \\(H\\)** of \\(u\\) and \\(v\\), respectively. Since these are neighbors in \\(H\\), \\(d_H(u,v)\leq d_H(x,y)+2\\). If we can construct \\(H\\) such that: $$d_H(x,y) \leq d_G(u,v) +4,$$ then we will have additive stretch +6. See Figure 1(b).  The key idea is to look at the number of missing edges of \\(P_G(u,v)\\). 
 

4. Suppose that  \\(P_G(u,v)\\) has about \\(2^i\\) missing edges for some \\(i\\). An edge is *missing* if it is not in \\(H\\); a missing edge is incident to high-degree vertices only. If  we sample vertices with probability about \\(\frac{\log n}{n^{1/3}2^i}\\) (the set \\(D_i\\) in line 5 below) we catch at least one vertex, say \\(z\\), in the neighborhood of some high degree vertex \\(t \in P_G(u,v)\\). Note that the two paths \\(Q_1 =(z,t)\circ P_G(t,u) \circ (u,x)\\) and  \\(Q_2 = (z,t)\circ P_G(t,v)\circ (v,y)\\) miss at most \\(2^i\\) edges each, and we want to catch these paths. We do so by adding to  \\(H\\), for every vertex \\(s\in S\\), a **shortest paths missing at most \\(2^i\\)** from \\(z\\) to \\(s\\). These paths will include the two paths \\(Q_1'\\) and \\(Q_2'\\) from \\(z\\) to \\(x\\) and \\(y\\), respectively, each has at most \\(2^i\\) missing edges. The main observation is that \\(\lvert Q_1'\rvert \leq \lvert Q_1\rvert\\) and \\(\lvert Q_2'\rvert \leq \lvert Q_2\rvert\\), and therefore,  $$d_H(x,y) \leq \lvert Q_1'\rvert + \lvert Q_2'\rvert\leq \lvert Q_1\rvert + \lvert Q_2\rvert = d_G(u,v) + 4,$$ as we want in step 3. The total number of edges added in this step will be $$\frac{n\log n}{n^{1/3}2^i}\cdot (\lvert S\rvert \cdot 2^i) = n^{2/3}\log n\cdot \tilde{O}(n^{2/3}) = \tilde{O}(n^{4/3})$$


5. The number of edges \\(\tilde{O}(n^{4/3})\\) is to cover for a specfic number of missing edges \\(2^i\\) for some \\(i\\). To cover all possible values of missing edges, we only need to consider values of \\(i\\) from \\(0\\) up to \\(\log n\\) (the loop in line 4 below). In the end, the number of edges of the spanner is blown up only by a factor of \\(O(\log n)\\).

The pseudo-code of the algorithm is described below. We say that an edge is **missing** if both of its endpoints are high-degree. (We do not define a missing edge as an edge not in \\(H\\) since \\(H\\) changes over time.) 

***
 <span style="font-variant: small-caps">Additive6Spanner</span>\\((G = (V,E))\\)
> 1. \\(H \leftarrow (V,\emptyset)\\)
> 2. add to \\(H\\) edges incident to vertices of degree at most \\(n^{1/3}\\)
> 3. \\(S\leftarrow\\) sample every vertex with prob. \\(\frac{10\log n}{n^{1/3}}\\)
> 4. for \\(i\leftarrow 0\\) to \\(\log n\\) 
> 5.  &nbsp;&nbsp;&nbsp;&nbsp; \\(D_i\leftarrow\\)   sample every vertex with prob. \\(\frac{10\log n}{n^{1/3}2^i}\\)
> 6. &nbsp;&nbsp;&nbsp;&nbsp; for every \\(z\in D_i\\)
> 7. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; for every \\(x\in S\\)
> 8. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; \\(\mathcal{P}(x,z,2^i)\leftarrow\\) all paths from \\(z\\) to \\(x\\) in \\(G\\) missing \\(\leq 2^{i}\\) edges in \\(H\\)
> 9. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; add shortest path in \\(\mathcal{P}(x,z,2^i)\\) to \\(H\\)
> 10. return \\(H\\) 

***


**Number of edges.** The number of edges added in line 2 is \\(O(n^{4/3})\\).  Observe that \\(\lvert S \rvert= \tilde{O}(n^{2/3})\\), and that \\(\lvert D_i\rvert = \frac{n\log n}{n^{1/3}2^i}\\). Since the path added in line 9 only has \\(2^{i}\\) missing edges, the total number of edges added in the for loop in line 6 for every fixed \\(i\\) is:

$$\frac{n\log n}{n^{1/3}2^i}\cdot (\lvert S\rvert \cdot 2^i) = n^{2/3}\log n\cdot \tilde{O}(n^{2/3}) = \tilde{O}(n^{4/3})$$

Thus, the total number of edges in \\(H\\) remains \\(\tilde{O}(n^{4/3})\\).

**The additive stretch.** Let \\(s\\) and \\(t\\) be any two vertices. If \\(P_G(s,t)\\) has no high-degree vertex, then it is in \\(H\\) and the additive stretch for pair \\((s,t)\\) is \\(0\\). Otherwise, let \\(u\\) and \\(v\\) be the high-degree vertices in  \\(P_G(s,t)\\) closest to \\(s\\) and \\(t\\), respectively. Note that edges of \\(P_G(s,u)\\) and \\(P_G(t,v)\\) are in \\(H\\). Thus, we can show that the additive stretch of pair \\((u,v)\\) is \\(+6\\), then the additive stretch of pair \\((s,t)\\) is also \\(+6\\).

Let's focus on  \\(P_G(u,v)\\). By the same argument in Claim 1, w.h.p, there exists \\(x,y \in S\\) that are neighbors in \\(H\\) of \\(u\\) and \\(v\\), respectively. By the triangle inequality, \\(d_H(u,v) \leq d_H(x,y)+ 2\\). Next, we will show that 

$$d_H(x,y) \leq d_G(u,v) + 4$$

If so, then \\(d_H(u,v) \leq d_G(u,v)+ 6\\) as desired.

Let \\(i \in [0,\log n]\\) be such that the number of missing edges on \\(P_G(u,v)\\) is at most \\(2^{i}\\) and at least \\(2^{i-1}\\). 

***
**Claim 2**: Let \\(P\\) be a shortest path in \\(G\\). Then there is no vertex in \\(G\\) that has more than 3 neighbors in \\(P\\). 

***

**Proof:**  If there is such a vertex, then we can shortcut \\(P\\) through that vertex to get a shorter path, contradicting that \\(P\\) is a shortest path.

***

By Claim 2 and the fact that the endpoints of a missing edge are high-degree, the number of vertices having at least one neighbor on  \\(P_G(u,v)\\) is \\(\Omega(2^{i} n^{1/3})\\). Thus, w.h.p, \\(D_i\\) contains such a vertex \\(z\\). Let \\(t \in P_G(u,v)\\) be a neighbor of \\(z\\). Let \\(Q_1 =(z,t)\circ P_G(t,u) \circ (u,x)\\) and  \\(Q_2 = (z,t)\circ P_G(t,v)\circ (v,y)\\). Both \\(Q_1\\) and \\(Q_2\\) are paths from \\(z\\) to vertices in \\(S\\), each has at most \\(2^i\\) edges missing. Thus, by line 9 of the algorithm, we have:

$$ d_H(z,x) \leq \lvert Q_1\rvert = d_G(u,t) + 2$$

$$d_H(z,y) \leq \lvert Q_2\rvert = d_G(t,v) + 2$$ 

Thus, we have \\(d_H(x,y) \leq d_G(u,v)+ 4\\) as desired. 


**Remark 2:** It is not obvious how one can find a path in line 9 efficiently. Indeed, such a path can be found by dynamic programming; we refer readers to the paper by [Woodruff](https://www.cs.cmu.edu/afs/cs/user/dwoodruf/www/w10.pdf) for details.

**Remark 3:** One can find a \\(+6\\) spanner with \\(O(n^{4/3})\\) edges, either by the clever path-buying framework of [Baswana, Kavitha, Mehlhorn, and Pettie](https://web.eecs.umich.edu/~pettie/papers/spanner-journal.pdf) or a simple greedy algorithm by [Knudsen](https://arxiv.org/abs/1403.0178). 



# Additive +4- and +k-Spanners

The situation for \\(+4\\)-spanners is very interesting. We know that a \\(+4\\)-spanner should have the number of edges between \\([n^{4/3}, n^{3/2}]\\). [Chechik](https://u.cs.biu.ac.il/~rodittl/spanner-shiri.pdf) constructed a \\(+4\\)-spanner with \\(\tilde{O}(n^{7/5})\\) edges, implying that  \\(n^{3/2}\\) is not the right bound. However, determining the correct bound remains an outstanding open problem; a popular belief is \\(n^{4/3}\\). 


***
**Open Problem 1**: Construct a \\(+4\\)-spanner with \\(O(n^{3/4})\\) edges.

***

Another related open problem (pointed out by Greg Bodwin) which looks somewhat more naive but implies Open Problem 1 is this:

***
**Open Problem 2**: Prove for any set $S$ of $\lvert S\rvert \geq n^{2/3}$ nodes, there is a subgraph on $O(\lvert S \rvert^2)$ edges that preserves all distances within $S$.

***


How about +8 or +10-spanners? For a while, it was believed that the number of edges of a \\(+(2k-2)\\) should be about \\(n^{1+1/k}\\) for any integer constant \\(k\\), until the stunning work of [Abboud and Bodwin](https://arxiv.org/pdf/1511.00700), showing that \\(n^{4/3}\\) is the limit. Informally,  they showed that for any constant additive stretch, the minimum number of edges is \\(\Omega(n^{4/3-\epsilon})\\). Formally:


***
**Theorem 1** ([Abboud and Bodwin](https://arxiv.org/pdf/1511.00700)): For every \\(\epsilon > 0\\), there is a \\(\delta  > 0\\) and a graph \\(G\\) with \\(n\\) vertices such that for any subgraph \\(H\\) with \\(O(n^{4/3-\epsilon})\\) edges:

$$d_H(u,v) = d_G(u,v) + \Omega(n^{\delta})$$

for some pair of vertices \\(u,v\\).

***


Therefore, for the constant stretch regime, the open problem about +4-spanners is the only piece left to complete the picture. 

For the non-constant stretch regime, there has been a long line of work, which should be the topic of another post.

# Our Result: Weighted Additive Spanners

Now it's time to talk about what we did. We looked at additive spanners for weighted graphs. Two natural definitions of weighted additive spanners:

1. \\(+k W_{\max}\\) where \\(W_{\max}\\) is the maximum edge weight. That is, \\(d_H(u,v)\leq d_G(u,v) +k W_{\max}\\) for every \\(u,v\in V(G)\\).  This definition was introduced by [Ahmed, Bodwin, Sahneh, Kobourov, and  Spence.](https://arxiv.org/abs/2002.07152)
2.  \\(+k W_{u,v}\\) where the additive stretch depends on the vertex pair, called *local error*. That is: \\(d_H(u,v)\leq d_G(u,v) +k W_{u,v}\\) where \\(W_{u,v}\\) is the maximum edge weight on the shortest path from \\(u\\) to \\(v\\) in \\(G\\). This definition is clearly stronger than the former one and was introduced by [Ahmed, Bodwin, Hamm, Kobourov, and Spence](https://arxiv.org/abs/2103.09731). 


As pointed out by [Ahmed et al.](https://arxiv.org/abs/2002.07152), results for unweighted graphs could not be easily translated to weighted graphs due to a technical reason: after adding arbitrary \\(d\\) edges per vertex to a spanner, a shortest path in unweighted graphs missing \\(\ell\\) edges (in the current spanner) has \\(\Omega(d\ell)\\) neighbors, while the same fact does not hold for weighted graphs. They showed, with clever proof, that adding  \\(d\\) *lightest edges* per vertex for the weighted case fixes the problem.

Adding \\(d\\) lightest edges per vertex for an appropriate value of \\(d\\) is the key to all the work involving weighted additive spanners. Soon, bounds for the unweighted case are recovered: [\\(+2W_{\max}\\)](https://arxiv.org/abs/2002.07152) and [\\(+2 W_{u,v}\\)](https://arxiv.org/abs/1907.11422) spanners with \\(O(n^{3/2})\\) edges; [\\(+4W_{\max}\\)](https://arxiv.org/abs/2103.09731)  spanners with \\(\tilde{O}(n^{7/5})\\) edges.

Our work is about \\(+6W_{\max}\\) and \\(+6W_{u,v}\\) spanners. Prior to our work, [Elkin, Gitlitz, and Neiman](https://arxiv.org/abs/2008.09877) constructed a \\(+(6+\epsilon)W_{u,v}\\) with \\(O(n^{4/3}/\epsilon)\\) edges. Is the dependency on \\(\epsilon\\) necessary? We show that it is not:


***
**Theorem 2**: For any edge-weighted graph \\(G\\), one can construct a \\(+6W_{u,v}\\) spanner with \\(\tilde{O}(n^{4/3})\\) edges. 

***

To construct the spanner in Theorem 2, we adapt the randomized approach by Woodruff for constructing a +6-spanner above to the weighted setting: we replace the 2nd step with a \\(n^{1/3}\\)-light initialization. We observe that the rest of the analysis carries over the weighted setting. Based on this adaptation, we also presented several fast constructions of weighted additive spanners. Check out our work [here](https://arxiv.org/pdf/2408.14638). 


