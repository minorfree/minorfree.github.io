---
title:  "Halperin-Zwick Algorithm for Spanners"
mathjax: true
layout: post
categories: media
---

This post describes a simple linear time algorithm by Halperin-Zwick [6] for constructing a \\((2k+1)\\)-spanner of unweighted graphs for any given integer \\(k\geq 1\\). The spanner  has \\(O(n^{1+1/k})\\) edges and hence is sparse. When \\(k = \log n\\), it only has \\(O(n)\\) edges.  This sparsity makes spanners an important object in many applications.

This post was inspired by my past attempt to track down the detail of the Halperin-Zwick algorithm. Halperin and Zwick never published their algorithm, and all papers I am aware of cite their unpublished manuscript [6]. The algorithm by Halperin-Zwick is a simple modification of an earlier algorithm by Pelege and Schäffer [8], which, according to Uri Zwick, is the reason why they did not publish their result. The spanner by Pelege and Schäffer [8] has stretch \\(4k+1\\) for the same sparsity. The idea of Halperin-Zwick algorithm was given as Exercise 3 in Chapter 16 of  the book by Peleg [7].

First, let's define spanners. Graphs in this post are connected.

***
**\\(t\\)-Spanner**: Given a graph \\(G\\), a \\(t\\)-spanner is a subgraph of \\(G\\), denoted by \\(H\\),  such that for every two vertices \\(u,v\in V(G)\\):

$$ d_H(u,v)\leq t\cdot d_G(u,v)$$

***

Here \\(d_H\\) and \\(d_G\\) denote the graph distances in \\(H\\) and in \\(G\\), respectively. Graph \\(G\\) could be weighted or unweighted; we only consider unweighted graphs in this post. The distance constraint on \\(H\\) implies that \\(H\\) is connected and spanning. 

Parameter \\(t\\) is called the *stretch* of the spanner. We often construct a spanner with an odd stretch: \\(t = 2k+1\\) for some integer \\(k\geq 1\\). Why not even stretches? Short answer: there is no gain in terms of the worst case bounds for even stretch [1].


***
**Theorem** (Halperin-Zwick): Let \\(G\\) be an unweighted graph with \\(n\\) vertices and \\(m\\) edges. Let \\(k\geq 1\\) be any given integer. There is an algorithm that runs in time \\(O(m)\\) and constructs a \\((2k+1)\\)-spanner of \\(G\\) with \\(O(n^{1+1/k})\\) edges.

***

It is often instructive to think about \\(k=1\\), i.e, constructing a \\(3\\)-spanner. And this is where we start. 

# Stretch 3

Here we seek a \\(3\\)-spanner with \\(O(n^{3/2})\\) edges. There are two steps: clustering and connecting the clusters. Let's focus on clustering first. The idea is to: construct a set of radius-1 clusters (a set of stars) that have at least \\(\sqrt{n}\\) vertices each. This implies that the number of clusters is \\(O(\sqrt{n})\\) and hence we can afford to add one edge from each vertex to each cluster. The remaining vertices induce a graph of at most \\(O(n^{3/2})\\); we can add all the edges.

The cluster can be constructed greedily; the pseudocode of the algorithm is given below. We use \\(N_G(v)\\) to denote neighbors of \\(v\\) in a graph \\(G\\). 

***
 <span style="font-variant: small-caps">Clustering</span>\\((G)\\)
> \\(1.\\) \\({\mathcal C} \leftarrow \emptyset, \quad G_1\leftarrow G\\)<br>
> \\(2.\\) while \\(G_i \not= \emptyset\\)<br>
> \\(3.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\(x\leftarrow\\) an arbitrary vertex in \\(G_i\\)<br>
> \\(4.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\(C_x\leftarrow \{x\}\\)<br>
> \\(5.\\) &nbsp;&nbsp;&nbsp;&nbsp; if \\(\lvert N_{G_i}(x)\rvert  \geq \sqrt{n}\\)<br>
> \\(6.\\) &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;  \\(C_v\leftarrow C_v\cup N_{G_i}(x)\\)<br>
> \\(7.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\({\mathcal C} \leftarrow {\mathcal C}\cup \{C_x\}\\) <br>
> \\(8.\\) &nbsp;&nbsp;&nbsp;&nbsp;  \\(G_{i+1}\leftarrow G_i\setminus C_v, \quad i\leftarrow i+1\\)<br>
> \\(9.\\) return \\({\mathcal C}\\)

***
We call the vertex \\(v\\) in the cluster \\(C_v\\) in line 4 the *center* of the cluster. We use \\(E(C_v)\\) to the edges of \\(G\\) connecting \\(v\\) to other vertices in \\(C_v\\).

Observe  that every cluster \\(C\in {\mathcal C}\\) has radius at most \\(1\\) and it has either at least \\(\sqrt{n}\\) vertices or  exactly one vertex. We call \\(C\\) a *heavy cluster* if \\(\lvert C \rvert\geq \sqrt{n}\\), and a *light cluster* otherwise.  

***
**Observation 1**: The number of heavy clusters in \\({\mathcal C}\\) is at most \\(\sqrt{n}\\).

***

To get a 3-spanner of \\(G\\), we simply add an edge from every vertex to each heavy cluster of \\({\mathcal C}\\), and an edge between every pair of light clusters. (Light clusters are singletons.) 
***
 <span style="font-variant: small-caps">3Spanner</span>\\((G)\\)
> \\(1.\\) \\({\mathcal C} \leftarrow\\)<span style="font-variant: small-caps">Clustering</span>\\((G)\\)<br>
> \\(2.\\) \\(H\leftarrow (V,\emptyset)\\)<br>
> \\(3.\\) for each heavy cluster \\(C\in {\mathcal C}\\)<br>
> \\(4.\\) &nbsp;&nbsp;&nbsp;&nbsp; add \\(E(C)\\) to \\(H\\)<br>
> \\(5\\). &nbsp;&nbsp;&nbsp;&nbsp; for each vertex \\(v \in N_G(C)\\)<br>
> \\(6.\\) &nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; \\((v,u)\leftarrow\\) an arbitrary edge from \\(v\\) to \\(C\\)<br>
> \\(7.\\) &nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; add \\((u,v)\\) to \\(H\\)<br>
> \\(8.\\) add to \\(H\\) all edges between light clusters<br>
> \\(9.\\) return \\(H\\)

***

In line 5, we use \\(N_G(C)\\) to denote the set of neighbors of \\(C\\), which are vertices are not in \\(C\\) and having at least one edge to \\(C\\).  The running time is clearly \\(O(m)\\).

**Sparsity analysis.** Note that \\(E(C)\leq \lvert C \rvert-1\\). Thus, the total number of edges added to \\(H\\) in line 4 over all iterations is at most \\(n-1\\). Furthermore, the number of edges added to \\(H\\) in the loop in line 5 is at most \\(n\\) and hence by Observation 1, the total number of edges added in lines 3-7 is \\(O(n\sqrt{n}) = O(n^{3/2})\\). 

To bound the number of edges added in line 8, observe that, if we order light clusters by the order it is added to \\({\mathcal C}\\) in line 7 of algorithm <span style="font-variant: small-caps">Clustering</span>, then each light cluster is incident to at most \\(\sqrt{n}\\) light clusters following it in the order. It follows that the total number of edges added in line 8 is \\(O(n\sqrt{n}) = O(n^{3/2})\\).


**Stretch analysis.** We show that \\(d_G(u,v)\leq 3 d_H(u,v)\\). By the triangle inequality, it suffices to show the inequality for every edge \\((u,v)\\) of \\(G\\). This means we have to show that \\(d_H(u,v)\leq 3\\).  This inequality holds if \\((u,v)\in E(H)\\), and hence we only need to consider the case where at least one of \\(u\\) and \\(v\\) is in a heavy cluster.

![](/assets/figs/clusters.svg)

*Figure 1: (a) stretch-3 path for edge \\((u,v)\\) and (b) stretch-\\((2k+1)\\) path for edge \\((u,v)\\)*

If \\(u\\) and \\(v\\) are in the same heavy cluster \\(C\\), then \\(d_H(u,v)\leq 2\\) and the stretch guarantee holds. Otherwise, let \\(C_x\\) be the heavy cluster centered at \\(x\\) containing \\(v\\), say. As \\((u,v)\not\in H\\), there must be another vertex \\(w\in C_x\\) such that \\((u,w)\in H\\) by the construction in line 6. Thus, the path \\(u\rightarrow w\rightarrow x\rightarrow v\\) is a path of length 3 in \\(H\\) between \\(u\\) and \\(v\\), as desired. See Figure 1(a).


# Larger Stretches

The algorithm for constructing a \\((2k+1)\\)-spanner with \\(O(n^{1+1/k})\\) edges is somewhat similar to the stretch-3 case, but we will need a finer analysis. A key observation, which we also use in the 3-spanner construction, is that if we have a cluster, say \\(C\\), of radius \\(k\\), and a vertex \\(v\in N_G(C)\\), it suffices to keep only one edge from \\(v\\) to \\(C\\). Thus, as long as \\(N_G(C)\\) has at most \\(n^{1/k}\lvert C \rvert\\) vertices, we can add an edge from \\(v\\) to \\(C\\) for each \\(v\in N_G(C)\\); the *average number of edges added per vertex* of \\(C\\) is \\(n^{1/k}\\). 

What if \\(\lvert N_G(C) \rvert \geq n^{1/k}\lvert C \rvert\\)? In this case, we simply grow \\(C\\) by adding all of its neighbors. How many times will it grow? At most \\(k\\) times, as every time \\(C\\) grows, its size increases by a factor of \\(n^{1/k}\\), and there are only \\(n\\) vertices in the graph. 

The pseudocode of the algorithm is given below. The set \\(A\\) holds the edges between \\(C\\) and its neighbors described above. The rest is essentially the same as the clustering for stretch 3.

***
 <span style="font-variant: small-caps">Clustering</span>\\((G,k)\\)
> \\(1.\\) \\({\mathcal C} \leftarrow \emptyset, \quad A\leftarrow \emptyset, \quad G_1\leftarrow G\\)<br>
> \\(2.\\) while \\(G_i \not= \emptyset\\)<br>
> \\(3.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\(x\leftarrow\\) an arbitrary vertex in \\(G_i\\)<br>
> \\(4.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\(C_x\leftarrow \{x\}\\)<br>
> \\(5.\\) &nbsp;&nbsp;&nbsp;&nbsp; while \\(\lvert N_{G_i}(C_x)\rvert \geq n^{1/k} \lvert C_{x} \rvert\\)<br>
> \\(6.\\) &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;  \\(C_v\leftarrow C_x\cup N_{G_i}(C_x)\\)<br>
> \\(7.\\) &nbsp;&nbsp;&nbsp;&nbsp; for each \\(v \in N_{G_i}(C_x)\\)<br>
> \\(8.\\) &nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; \\((v,u)\leftarrow\\) an arbitrary edge from \\(v\\) to \\(C\\)<br>
> \\(9.\\) &nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; add \\((v,u)\\) to \\(A\\)<br>
> \\(10.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\({\mathcal C} \leftarrow {\mathcal C}\cup \{C_v\}\\) <br>
> \\(11.\\) &nbsp;&nbsp;&nbsp;&nbsp;  \\(G_{i+1}\leftarrow G_i\setminus C_v, \quad i\leftarrow i+1\\)<br>
> \\(12.\\) return \\(({\mathcal C},A)\\)

***

Once we perform clustering, we only need to add the set \\(A\\) and the edges inside each cluster to the spanner.

***
 <span style="font-variant: small-caps">Spanner</span>\\((G,k)\\)
> \\(1.\\) \\(H\leftarrow (V,\emptyset)\\)<br>
> \\(2.\\) \\(({\mathcal C},A) \leftarrow\\)<span style="font-variant: small-caps">Clustering</span>\\((G,k)\\)<br>
> \\(3.\\) add \\(A\\) to \\(H\\)<br>
> \\(4.\\) for each cluster \\(C\in {\mathcal C}\\)<br>
> \\(5.\\) &nbsp;&nbsp;&nbsp;&nbsp; add \\(E(C)\\) to \\(H\\)<br>
> \\(6.\\) return \\(H\\)

***

**Sparsity analysis.** The number of edges added in the loop in line 4 is at most \\(n-1\\). Observe that for each cluster \\(C_x\\) added to \\({\mathcal C}\\) in line 6 of <span style="font-variant: small-caps">Clustering</span>, the number of edges added to \\(A\\) in the loop in line 7 is at most \\(n^{1/k}\lvert C_{x} \rvert\\). Thus, \\(\lvert A \rvert\leq n^{1/k}\sum_{C}\lvert C \rvert \leq n^{1+1/k}\\). This implies that \\(\lvert E(H) \rvert = O(n^{1+1/k})\\).

**Stretch analysis.** Let \\((u,v)\\) be any edge of \\(G\\) such that \\((u,v)\not\in H\\). We need to show that \\(d_H(u,v)\leq 2k+1\\). Observe that:

***
**Observation 2**: Every cluster \\(C_x\in {\mathcal C}\\) has radius at most \\(k\\)

***
Proof: Every time the radius of \\(C_x\\) increases by \\(1\\), the size of \\(C_x\\) increases by a factor of \\(n^{1/k}\\) by the construction. Thus, after \\(t\\) rounds, \\(n\geq \lvert C_{x} \rvert\geq n^{t/k}\\), which gives \\(t\leq k\\).

***

Let \\(C_x\\) be the cluster containing \\(v\\). If \\(u\in C_x\\), then \\(d_G(u,v)\leq 2\cdot k\\). Otherwise, suppose w.l.o.g, that \\(v\\) is clustered before \\(u\\). Observe that \\(u\in N_{G_{i}}(C_x)\\) and hence an edge \\((u,w)\\) is added to \\(A\\), which is eventually added to \\(H\\). See Figure 1(b). Thus, the path consisting of an edge \\((u,w)\\), the shortest path from \\(w\\) to \\(x\\), and the shortest path from \\(x\\) to \\(v\\), is a path of length at most \\(2k+1\\) between \\(u\\) and \\(v\\) in \\(H\\), as desired.


# Optimality: The Girth Conjecture

It is not hard to construct a class of graphs such that for any graph \\(G\\) of size \\(n\\) in the class, the Halperin-Zwick algorithm produces a \\((2k+1)\\)-spanner for \\(G\\) that has \\(\Omega(n^{1+1/k})\\) edges. Could we go below the bound \\(\Theta(n^{1+1/k})\\) on the number of edges (by a different algorithm, say)? The consensus seems to be no, though currently we do not have a definite answer.

Spanners have a tight connection to the girth of graphs; a graph has girth \\(g\\) if the shortest simple cycle in the graph has length \\(g\\). 

***
**Observation 3**: Let \\(H\\) be a graph of girth \\(2k+3\\). Then any \\((2k+1)\\)-spanner of \\(H\\) must contain every edge of \\(H\\).

***

Observation 3 essentially says that any \\((2k+1)\\)-spanner of \\(H\\) must be itself. Thus, the question of the optimality of spanners reduces to: is there any graph with \\(o(n^{1+1/k})\\) edges and girth \\((2k+3)\\)? The Erdős' Girth Conjecture implies that the answer is no.

***
**Erdős' Girth Conjecture [5]**: For any \\(n \geq 1\\) and \\(k\geq 1\\), there exists a graph with \\(n\\) vertices of girth \\((2k+3)\\) that has \\(\Omega(n^{1+1/k})\\) edges.

***

Erdős stated a lower bound \\(c_k\cdot n^{1+1/k}\\) on the number of edges in the conjecture [5]; that is, the constant is allowed to degrade as \\(k\\) increases.  The spanner literature often cites the stronger version above, where the constant remains the same for every \\(k\\). The Erdős' Girth Conjecture is known to hold for a few small values of \\(k\\). 

While Erdős' Girth Conjecture remains wide open, we could ask: is it possible to construct a \\((2k+1)\\)-spanner that has girth at least \\(2k+3\\)? If yes, then the output spanner is (existentially) optimal regardless of the truth of Erdős' Girth Conjecture. 

It turns out that the following simple greedy algorithm, formally described in [2] and attributed to Marshall Bern, does the job: consider edges in increasing weight order and add an edge \\(e\\) to the current spanner if the distance between its endpoints in the spanner is larger than \\((2k+1)w(e)\\). The algorithm works for weighted graphs as well. It is an instructive exercise to show that the output graph is a \\((2k+1)\\)-spanner and has girth at least \\(2k+3\\).

The major downsize of the greedy algorithm is its running time: the current best known implemtation takes \\(O(mn^{1+1/k})\\) time. Even in unweighted graphs, to the best of my knowledge, the following problem remains open:

***
**Open Problem**: Construct a \\((2k+1)\\)-spanner of girth at least \\((2k+3)\\) in nearly linear time. 

***


# Conclusion

We have mentioned two algorithms for constructing a spanner. Another beautiful algorithm that I hope to cover in a future post is the randomized construction by Baswana and Sen [4]. A notable feature of the Baswana-Sen algorithm is that it can be implemented efficiently in both parallel and distributed models. The recent survey paper [1] contains almost all known algorithms for spanners and its sibling problems. 

#  Bibliographical Notes

The concept of a spanner was formally introduced by Peleg and Schaffer [8], though its conception was much earlier. Peleg and Schaffer constructed a \\((4k-1)\\)-spanner with \\(O(n^{1+1/k})\\) edges by connecting every pair of clusters in the output of <span style="font-variant: small-caps">Clustering</span>\\((G,k)\\) by an edge. This clustering procedure was due to Awerbuch [2].




# References

[1] Ahmed, R., Bodwin, G., Sahneh, F. D., Hamm, K., Jebelli, M. J. L., Kobourov, S., and Spence, R. (2020). Graph spanners: A tutorial review. Computer Science Review, 37, 100253.

[2] Althöfer, I., Das, G., Dobkin, D., Joseph, D., and Soares, J. (1993). On sparse spanners of weighted graphs. Discrete \& Computational Geometry, 9(1), 81-100. 

[3] Awerbuch, B. (1985). Complexity of network synchronization. Journal of the ACM (JACM), 32(4), 804-823.

[4] Baswana, S., and Sen, S. (2007). A simple and linear time randomized algorithm for computing sparse spanners in weighted graphs. Random Structures & Algorithms, 30(4), 532-563.

[5] Erdős, P. (1965). Extremal problems in graph theory. In Proceedings of the Symposium on Theory of Graphs and its Applications, page 29-36, 1963.

[6] Halperin, S., and Zwick, U. (1996). Unpublished manuscript.

[7] Peleg, D. (2000). Distributed computing: a locality-sensitive approach. Society for Industrial and Applied Mathematics.

[8] Peleg, D., and Schäffer, A. A. (1989). Graph spanners. Journal of graph theory, 13(1), 99-116.

