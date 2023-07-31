---
title:  "Separator Theorems"
mathjax: true
layout: post
categories: media
---

The separator theorem for planar graphs  [Lipton and Tarjan](https://epubs.siam.org/doi/abs/10.1137/0136016) is the cornerstone of planar graph algorithms: any planar graph of \\(n\\) vertices can be separated into connected components of size at most \\(2(n)/3\\) each by removing \\(O(\sqrt{n})\\) vertices. (An earlier separator theorem by [Ungar](https://londmathsoc.onlinelibrary.wiley.com/doi/abs/10.1112/jlms/s1-26.4.256) has a slightly larger size of about \\(\sqrt{n}\log^{3/2}n\\).) The proof is beautiful but does rquire a non-trivial introduction to planar embedding and its properties, such as the [interdigitating tree](https://planarity.org/Klein_planar_embedded_graphs.pdf). My motivation for this post is to look for a proof that does not requires planar embedding. There are many other proofs of the (variants) of the planar separator theorem---see the [excellent book chapter](https://sarielhp.org/book/chapters/separator.pdf) by Sariel Har-Peled---but none fits into what I am looking for: a self-contained proof that does not require planar embedding. 

To be more precise, I look for a proof that only requires the input graph excluding a fixed minor, \\(K_5\\) for example in the case of planar graphs. There are two proofs that I particularly like: one by Plotkin, Rao, and Smith [2] and another by Alon, Seymour, and Thomas [1], which work for any graph excluding a fixed minor. I will present both. The central concept is a \\(K_h\\)-minor model.

***
**\\(K_h\\)-minor model**: a *\\(K_h\\)-minor model* of a graph \\(G\\) is a collection of \\(h\\) *vertex-disjoint connected* subgraphs \\({\mathcal K} = \{C_1,C_2,\ldots, C_h\}\\) of \\(G\\) such that there is an edge between any two subgraphs. Parameter \\(h\\) is called the model size.
***

![](/assets/figs/K5Model.svg)

*Figure 1: A graph (left) and its \\(K_5\\)-minor model (right).*

A graph is *\\(K_h\\)-minor-free* if it does not have a \\(K_h\\)-minor model. The [Kuratowski's theorem](https://en.wikipedia.org/wiki/Kuratowski%27s_theorem) implies that planar graphs are \\(K_5\\)-minor-free (and \\(K_{3,3}\\)-minor-free). Herein we assume that the input graph is \\(K_h\\)-minor-free, and think of \\(h\\) as a small constant. 

A *balanced separator* of a graph \\(G = (V,E)\\) is a subset of vertices \\(S\subseteq V\\) such that every connected component of \\(G\setminus S\\) has size at most \\(2n/3\\). There is nothing special about the constant \\(2/3\\); any constant smaller than \\(1\\), for example \\(4/5\\), works. In some places of this post, we will be handwavy on the constant. 

**Basic ideas**:   We will maintain a minor model \\({\mathcal K}\\) of size \\(r\\) for some \\(r\leq h-1\\). At each step, we either increase the model size by \\(1\\) or find a subset of vertices to add to the separator. This could be achieved, say, by considering a BFS tree rooted at some vertex \\(v\\), say \\(T_v\\). There are two cases:
1. \\(T_v\\) has low depth, then we add a subtree of \\(T_v\\) to the current minor model \\({\mathcal K}\\). Furthermore, the subtree has a small size since we only need to connect the root \\(v\\) to at most \\(h-1\\) other subgraphs, each by a (short) path in \\(T_v\\). 
2.  \\(T_v\\) has alarge depth, then some layer of \\(T_v\\) will have a small size, and we could add this layer to the separator.

In the end, we find either a \\(K_h\\)-minor model, certifying that the input graph is not \\(K_h\\)-minor-free, or a balanced separator of small size. We will also add some vertices in the current minor model to the separator and hence,  the fact that subgraphs of the minor model have a small size will help.

![](/assets/figs/ShallowTree.svg)

*Figure 2: (a) Found a tree of small depth rooted at a vertex \\(v\\), then (b) add a new subgraph to the current \\(K_3\\)-model to obtain a \\(K_4\\)-model. \(c\) The tree rooted at \\(v\\) has large depth, then (d) find a layer \\(X\\) of small size to add to the separator.*

A major subtlety in this basic strategy is to account for the size of the separator in the 2nd case, as the number of iterations could be very large. Two algorithms presented below have very different accounting techniques. Plotkin, Rao, and Smith [2] use charging: they charge the size of the separator (which basically is a BFS layer) to a smaller component after removing the separator, as the algorithm will recurse on the bigger component. Alon, Seymour, and Thomas [1] incorporated the separator directly to the minor model. The end result is that the algorithm by Plotkin, Rao, and Smith produces a larger separator by a factor of \\(\sqrt{\log(n)}\\) for a constant \\(h\\).


# Plotkin-Rao-Smith Algorithm

Let \\(H\\) be a subgraph of \\(G\\), and \\(C\\) be another subgraph of \\(G\\) such that \\(V(C)\cap H = \emptyset\\). We define \\(N_H(C)\\) as the set of vertices in \\(H\\) with neighbors in \\(C\\). For a subset of vertices \\(X\subseteq V(G)\\), denote by \\(\kappa_G(X)\\) the largest connected component of \\(G\setminus V(X)\\).

The algorithm, <span style="font-variant: small-caps">ShallowSeparator</span>, accepts an input graph \\(G\\) and a parameter \\(\ell\\). It either outputs a \\(K_h\\)-minor model where every subgraph in the model has depth \\(O(\ell \log(n))\\) (and hence are shallow), or produces a separator of small size depending on \\(\ell\\). The basic ideas were described above. In particular, lines 5-7 implement case 1: adding a subgraph of low diameter to the current minor model \\({\mathcal K}\\). The condition \\(C_v\cap N_G(C)\not=\emptyset \quad\forall C\in {\mathcal K}\\) in line 6 guarantees that \\(C_v\\) has an edge to every subgraph currently in \\({\mathcal K}\\). Observe that \\(C_v\\) has size \\(O(h \ell \log (n))\\) as \\(C_v\\) includes at most \\(\lvert{\mathcal K}\rvert\leq h-1\\) paths of length at most \\(2\ell \log(n)\\) paths. (In the worst case \\(C_v\\) includes \\(\lvert{\mathcal K}\lvert\\) paths rooted at \\(v\\), each connect \\(v\\) to a subset \\(N_G(C)\\) for each \\(C\in {\mathcal K}\\).)  

Lines 10-14 implement the second case: find a separator \\(X\\) of \\(H\\) of small size. As the algorithm continues on the larger component (line 13), we would like to charge the size of \\(X\\) to all vertices not in the largest component. The condition in line 11 guarantees that each vertex outside the largest component receives at most \\(1/\ell\\) charges, which then finally contributes a factor of \\(O(n/\ell)\\) to the size of the separator. As vertices in the largest components were not charged, we could charge to them in later iterations. 

When we recurse on the largest component \\(H\\) in line 13, we only keep subgraphs in \\({\mathcal K}\\) that have edges to vertices \\(H\\); those that do not have edges in \\(H\\) should be removed from \\({\mathcal K}\\). The prodedure <span style="font-variant: small-caps">Trim</span>\\(({\mathcal K},H)\\) does exactly that: remove subgraphs from \\({\mathcal K}\\) not having edges to \\(H\\).


***
 <span style="font-variant: small-caps">ShallowSeparator</span>\\((G,\ell)\\)
> \\(1.\\) \\({\mathcal K}\leftarrow \emptyset\\), \\(S\leftarrow \emptyset\\), and \\(H\leftarrow G\\)<br>
> \\(2.\\) while \\(V(H)\geq 2n/3\\) <br>
> \\(3.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\(v\leftarrow\\) arbitrary vertex in \\(V(H)\\)<br>
> \\(4.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\(T_{v}\leftarrow\\)<span style="font-variant: small-caps">BFSTree</span>\\((v,H)\\)<br>
> \\(5.\\) &nbsp;&nbsp;&nbsp;&nbsp; if \\(\mathrm{depth}(T_v) \leq 2\ell \ln(n)\\)<br>
> \\(6.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(C_v\leftarrow\\) minimal subtree of \\(T_v\\) s.t \\(C_v\cap N_H(C)\not=\emptyset \quad\forall C\in {\mathcal K}\\)<br>
> \\(7.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\({\mathcal K}\leftarrow {\mathcal K}\cup \{C_v\}\\) 
> \\(8.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp;  return \\({\mathcal K}\\) if \\(\lvert{\mathcal K}\rvert = h\\)<br> 
> \\(9.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(H\leftarrow \kappa_H(V(C_X))\\)<br>
> \\(10.\\) &nbsp;&nbsp;&nbsp;&nbsp; else<br>
> \\(11.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; Find \\(X\\) s.t \\(0 < \lvert X\rvert \leq \frac{\lvert V(H)\rvert - \lvert\kappa_H(X)\rvert} {\ell}\\)<br>
> \\(12.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(S\leftarrow S\cup X\\)<br>
> \\(13.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(H\leftarrow \kappa_H(X)\\)<br>
> \\(14.\\)  &nbsp;&nbsp;&nbsp;&nbsp; \\({\mathcal K}\leftarrow\\)<span style="font-variant: small-caps">Trim</span>\\(({\mathcal K},H)\\) \\(\qquad \ll \text{remove subgraphs not adjaent to }H \gg\\) <br>
> \\(15.\\) \\(S\leftarrow S\cup (\cup_{C\in {\mathcal K}} V(C))\\)<br>
> \\(16.\\) return \\(S\\) 

***

Observe that, as long as the algorithm can find \\(X\\) in line 11, it eventually will terminate since in every step, the size of \\(H\\) is reduced by at least \\(1\\).

It is not clear why \\(X\\) exists in the above algorithm; we will return to this issue later. The important thing to keep in mind is  the depth threshold of \\(2\ell \log(n)\\) in line 5.  We now show that the separator has small size. The intuition of the proof was already discussed above. First, we show several properties of \\(H\\) and \\({\mathcal K}\\); one of which implies that \\({\mathcal K}\\) is a clique minor model in every step of the while loop. 




***
**Lemma 1**: In every iteration of the while loop: (1) no subgraph in \\({\mathcal K}\\) shares a vertex with \\(H\\), (2) every subgraph in \\({\mathcal K}\\) has an edge to \\(H\\), and (3) every two graphs in \\({\mathcal K}\\) are vertex-disjoint and have an edge in \\(G\\) connecting them.

***

Proof:  We prove all four claims by induction; initially, \\({\mathcal K} = \emptyset\\) and hence the lemma trivially holds. There are two places where \\({\mathcal K}\\) and \\(H\\) potentially change in each iteration: lines 7,9,13 and 14. 

In line 7, we add \\(C_v\\) to \\({\mathcal K}\\). Line 6 and the induction hypothesis on claims (1) and (2) guarantee that (3) holds for \\({\mathcal K}\\) in the next iteration.  

In lines 9 and 13, \\(H\\) is updated, and in both cases, \\(H\\) is vertex-disjoint from \\({\mathcal K}\\), and hence (1) holds in the next iteration. 

Finally, in line 14, the <span style="font-variant: small-caps">Trim</span> procedure guarantees (2).
 
***

We now bound the size of the separator. 

***
**Theorem 1**: For any integer \\(\ell\geq 1\\), <span style="font-variant: small-caps">ShallowSeparator</span>\\((G,\ell)\\) either returns a \\(K_h\\)-minor model of \\(G\\) where every subgraph in the model has diameter at most \\(O(\ell \log n)\\) or a *balanced* separator \\(S\\) such that:
$$\lvert S\rvert = O\left(\frac{n}{\ell} + h^2\ell\log(n)\right)$$

***

Proof: Observe that if the algorithm returns  \\({\mathcal K}\\) in line 8, claim (3) in Lemma 1 implies that \\({\mathcal K}\\) is a \\(K_h\\)-minor-model. Furthermore, every subgraph in \\({\mathcal K}\\) has diameter \\(O(\ell \log n)\\), as the depth of \\(C_v\\) in line 6 is \\(O(\ell \log n)\\). Henceforth, we assume that the algorithm does return a separator \\(S\\) in line 16; this means \\(\lvert{\mathcal K}\rvert\leq h-1\\) at every iteration.


Whenever we add \\(X\\) to the separator \\(S\\) in line 12, we   can charge the size of \\(X\\) to vertices in \\(H\setminus \kappa_H(x)\\), each gets \\(1/\ell\\) charge. As we continue on \\(\kappa_H(X)\\) in line 13, each vertex will be charged at most once during the execution of the algorithm. Thus, the total number of vertices added to \\(S\\) in line 12 over the course of the while loop is at most \\(n/\ell\\).


Now we bound the vertices added in line 14. We define \\(V({\mathcal K}) = (\cup_{C\in {\mathcal K}} V(C))\\).  Whenever we add a subgraph \\(C_v\\) to \\({\mathcal K}\\), \\(C_v\\) has size \\(O(h\ell \log(n))\\) as observed above, and furtermore, \\(C_v\\) will never change. Thus, in line 14, \\(\lvert V({\mathcal K})\rvert = O(h^2 \ell \log(n))\\), implying the bound on the size of the separator. 

Finally, we show that \\(S\\) is balanced: every connected component of \\(G[V\setminus S]\\) has size at most \\(2n/3\\). Let \\(H_0\\) be \\(H\\) in the final iteration; \\(V(H_0)\geq 2n/3\\). In this iteration, we set \\(H = \kappa_{H_0}(X)\\) and that \\(\lvert V(H)\rvert\leq 2n/3\\). By induction, one could show that:

> Claim 1: \\({\mathcal K}\\) and \\(S\\) (at any iteration) form a separator of \\(H\\). That is, the neighbor (in \\(G\\)) of every vertex \\(v\in H\\) is either in \\(H\\), \\(V({\mathcal K})\\) or \\(S\\). 

Let \\(S_0\\) be \\(S\\) in the last iteration and before the update in line 12. The final separator will be \\(S = V({\mathcal K})\cup S_0 \cup X\\). 

Let \\(Z\\) be any connected component of \\(G[V\setminus S]\\). If \\(V(Z)\cap V(H_0) = \emptyset\\), then \\(\lvert V(Z)\rvert\leq n/3\\) as \\(\lvert V(H_0)\rvert\geq 2n/3\\). If not, then by Claim 1, \\(Z\\) is a connected component of \\(H_0\setminus X\\). Since \\(H\\) is the largest component of \\(H_0\setminus X\\) and it has size at most \\(2n/3\\), \\(\lvert V(Z)\rvert\leq 2n/3\\) as desired.   

***


By choosing \\(\ell = \sqrt{n \log(n)}/h\\), we obtain the following corollary:

***
**Corollary 1**: Any \\(K_h\\)-minor-free graph of \\(n\\) vertices admits a balanced separator of size \\(O(h\sqrt{n \log n})\\).

***

Note that the proof of Theorem 1 implicitly assumes that \\(X\\) in line 11 always exists, which we now show.

To get some intuition, consider the following simple algorithm: visit \\(T_v\\) layer by layer, starting from the root \\(v\\) (at layer \\(0\\)), and stop whenever we encounter a layer of small size: its size is at most \\(1/\ell\\) fraction of all previous layers. More formally, let \\(Y_{j}\\) be the set of all vertices at layer \\(j\\). Then we will stop at layer \\(j^{\ast}\\) where: 

$$\lvert Y_{j^{\ast}}\rvert \leq \frac{1}{\ell}\sum_{j \leq j^{\ast}-1}\lvert Y_{j}\rvert \qquad (1)$$  

If there is no such \\(j^{\ast}\\), then this means every time we visit a layer \\(j\\), the total size of all layers from \\(0\\) to \\(j\\)  grows by a factor of \\((1+ \frac{1}{\ell})\\) compared to the total size of all layers from \\(0\\) to \\(j-1\\). Thus, we have:

$$n \geq \sum_{j \leq \mathrm{depth}(T_v)}\lvert Y_{j}\rvert \geq \left(1+ \frac{1}{\ell}\right)^{\mathrm{depth}(T_v)}$$

which gives \\(\mathrm{depth}(T_v)\leq \ell\ln(n)\\).


We could set \\(X = Y_{j^{\ast}}\\). If the largest component \\(\kappa(X)\\) belongs to levels larger than \\(j^{\ast}\\), that is \\(\kappa(X)\subseteq (\cup_{j\geq j^{\ast}+1}Y_j)\\), then we are done since \\(\sum_{j \leq j^{\ast}-1}\lvert Y_{j}\rvert\leq \lvert V(H)\rvert- \lvert\kappa(X)\rvert\\). However, this might not be the case.  A simple fix is to find a layer \\(j^{\ast}\\) such that:

$$\lvert Y_{j^{\ast}}\rvert  \leq \frac{1}{\ell}\sum_{j \leq j^{\ast}-1}\lvert Y_{j}\rvert \qquad \& \qquad 
    \lvert Y_{j^{\ast}}\rvert  \leq \frac{1}{\ell}\sum_{j \geq j^{\ast}+1}\lvert Y_{j}\rvert \qquad (2)$$

Thus, regarless of whether the largest component \\(\kappa(X)\\) belongs to levels larger or smaller than \\(j^{\ast}\\), we always have \\(\lvert X\rvert =\lvert Y_{j^{\ast}}\rvert\leq (\lvert V(H)\rvert- \lvert\kappa(X)\rvert)/\ell\\). 

To find \\(j^{\ast}\\) satisfying Equation (2), we simply check each layer of \\(T_v\\). We show that if we could not find such a layer \\(j^{\ast}\\), then the depth of \\(T_v\\) is small.

***
**Lemma 2**: If \\(\mathrm{depth}(T_v)> 2\ell\ln(n)\\), then there exists a layer \\(j^{\ast}\\) of \\(T_v\\) satisfying Equation (2).

***

Proof: We visit \\(T_v\\) layer by layer, starting from layer \\(0\\) (the root). We mark a layer \\(j\\) *red* if \\(\lvert Y_{j}\rvert  \geq \frac{1}{\ell}\sum_{t \leq j-1}\lvert Y_{t}\lvert\\) and *blue* if \\(\lvert Y_{j}\rvert  \geq \frac{1}{\ell}\sum_{t \geq j+1}\lvert Y_{j}\lvert\\). (If a layer could be marked both red or blue, we mark it red only.) Intuitively, whenever we encounter a red layer, then the set of vertices seen so far grows by a factor of \\((1+1/\ell)\\). In contrast, whenever we encounter a blue layer, the set of the vertices we **have not** seen so far reduces by a factor of \\((1+1/\ell)\\). 


If there is an uncolored layer, then we found \\(j^{\ast}\\) satisfying Equation (2). Thus, we assume that every layer is colored. Our goal is to show that \\(\mathrm{depth}(T_v)\geq 2\ell\ln(n)\\).

First, we claim that there are at most \\(\ln(n)/\ell\\) red layers. Let \\(Z_{j} = \sum_{t \leq j}\lvert Y_{t}\lvert\\). For every two consecutive red layer \\(a\\) and \\(b\\) where \\(b \geq a+1\\), then 

$$Z_{b} \geq (1+1/\ell)\frac{1}{\ell}\sum_{t \leq b}\lvert Y_{t}\rvert \geq (1+1/\ell) Z_{a}~.$$

That is, every time we see a red layer, the size of all the layers up to the red layer increases by a factor of \\((1+1/\ell)\\), which implies that the number of red layers is at most \\(\ell\ln(n)\\).

By the same argument, one could show that the number of blue layers is at most  \\(\ell\ln(n)\\): every time we see a blue layer, the number of vertices in the blue and higher layers is reduced by a factor of \\((1-1/\ell)\\). Thus, the number of blue layers is at most \\(\ell\ln(n)\\), implying the bound on the depth.

***


**Remark:** if one optimizes the constant, the size of the separator is \\(n/\ell + 2(h-1)(h-2)\ell \ln(n)\\). For planar graphs, \\(h = 5\\) and the size of the separator is \\(2\sqrt{6n\ln(n)}\\) for an appropriate choice of \\(\ell\\).

# Alon-Seymour-Thomas  Algorithm


The Plotkin-Rao-Smith (PRS) algorithm maintains a model  \\({\mathcal K}\\)  where each subgraph in \\({\mathcal K}\\) is a tree of low depth. In every step, the algorithm either adds a subset of vertices to the (current) separator \\(S\\) or adds new vertices to \\({\mathcal K}\\). The final separator is the union of the current separator and all the vertices in the minor model \\({\mathcal K}\\);  a charging argument is used to bound \\(\lvert S\lvert\\).


In contrast, Alon-Seymour-Thomas (AST) algorithm only maintains \\({\mathcal K}\\). Every subgraph \\(C\in {\mathcal K}\\) either has low depth ( of \\(\ell\\)) or **has at most \\(n/\ell\\) neighboring vertices in the current largest component**, or more formally,  \\(\lvert N_H(C)\rvert \leq n/\ell\\) where \\(H\\) is the current largest component. The final separator contains either \\(C\\)  or \\(N_H(C)\\), whichever has a smaller size. In the former case, \\(C\\) has size  \\(O(h\ell)\\), and in the latter case, \\(N_H(C)\\)  has size \\(O(n/\ell)\\). Thus, the size of the separator is \\(O(h(h\ell + n/\ell)) = O(h^{3/2}\sqrt{n})\\) for the choice of \\(\ell = \sqrt{n/h}\\).

The following lemma is the key to the algorithm whose proof we will delay. It is an interesting exercise though. 

***
**Lemma 2**: Let \\(\ell \leq 1\\) be any parameter. Let \\(A_1,A_,2\ldots, A_k\\) be \\(k\\) subsets of vertices in \\(V\\). There exists either:
- (i) a tree \\(T\\) of \\(G\\) of size at most \\(\ell k\\) such that \\(V(T)\cap A_i \not= 0\\) for every \\(i\in [1,k]\\) 
- or (ii) a set \\(X\\) of size at most \\(n/\ell\\) such that for every connected component \\(C\in G\setminus X\\), \\(X\cap A_i=\emptyset\\) for some \\(i\in [k]\\).

***

Intuitively, Lemma 2 means that either we find a tree of small size that connects every set \\(A_i\\) or we find a small separator that separates at least two sets among \\(\{A_i\}_i\\) into two different connected components.  We note that \\(\{A_i\}_i\\) might not be vertex-disjoint.

> **Remark 1:** A special case that helps understand Lemma 2 is when each \\(A_i\\) is a single vertex. In this case, we could simply find a BFS tree starting from a vertex \\(v = A_1\\). If \\(d_G(A_i,v)\leq \ell\\), then we simply truncate the tree at level \\(\ell\\) to get item (i). Otherwise, the separator is the level of smallest size among \\(\ell\\) lelvels from \\(1\\) to \\(\ell\\), which has size at most \\(n/\ell\\).





We need more notation to describe the algorithm formally. For a subset of vertices \\(A\in V(H)\\), let \\(\mathrm{Reach}(A,H)\\) be the set of all vertices that are reachable from \\(A\\) in \\(H\\); \\(H\\) might be disconnected. 

![](/assets/figs/ASTExtend.svg)

*Figure 3: (a) Found small separator \\(X\\), then (b) find a subgraph \\(B\\) in line 10 by extending the neighbor set of a subgraph \\(C\in {\mathcal K}\\), \(c\) extend \\(C\\) to include \\(B\\) and recurse on \\(\kappa_H(B)\\).*

At every iteration, the algorithm applies Lemma 2 to the sets of neighbors \\(N_H(C)\\) of subgraphs \\(C\\) in the model \\({\mathcal K}\\); this is procedure <span style="font-variant: small-caps">TreeeOrSep</span>\\((H, \\{N_H(C)\\}_{C\in {\mathcal K}})\\) in line 3. It will return a tree \\(T\\) of small size intersecting all these sets or a small separator \\(X\\) that separates at least two sets into two different components. In the former case, the algorithm will add \\(T\\) to the current model \\({\mathcal K}\\) and hence increase the size of  \\({\mathcal K}\\) by \\(1\\). In the latter case, there will be a subgraph \\(C\\) such that its neighbor set \\(N_H(C)\\) is disjoint from the largest component \\(\kappa_H(X)\\). (Recall that \\(\kappa_H(X)\\) is the largest component of \\(H\\) after removing \\(X\\) from  \\(H\\).) We then extend \\(C\\) in lines 10-11; the idea here is that after extending \\(C\\), \\(N_H(C)\\) will be a subset of \\(X\\) and hence \\(\lvert N_H(C)\rvert\leq \lvert X\rvert \leq n/\ell\\). See Figure 3. 

Finally, we recurse on the largest component \\(\kappa_H(B)\\) in line 12 where \\(B\\) is the set of vertices we added to \\(C\\). (We do not recurse on \\(\kappa_H(X)\\) since we do not add \\(X\\) to the separator.) As discussed above, the algorithm either returns \\(C\\) if it has small size or its neighbors in the current largest component. Note that some subgraphs in \\({\mathcal K}\\) might not be adjacent to \\(\kappa_H(B)\\), and hence we remove these subgraphs (line 13). 


***
 <span style="font-variant: small-caps">ASTSeparator</span>\\((G,\ell)\\)
> \\(1.\\) \\({\mathcal K}\leftarrow \emptyset\\), and \\(H\leftarrow G\\)<br>
> \\(2.\\) while \\(V(H)\geq 2n/3\\)<br> 
> \\(3.\\) &nbsp;&nbsp;&nbsp;&nbsp; \\((T,X)\leftarrow\\)<span style="font-variant: small-caps">TreeeOrSep</span> \\((H, \\{N_H(C)\\}_{C \in {\mathcal K}})\\) <br>
> \\(4.\\) &nbsp;&nbsp;&nbsp;&nbsp; if \\(\lvert V(T)\rvert \leq \ell k\\)<br>
> \\(5.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\({\mathcal K}\leftarrow {\mathcal K}\cup \{T\}\\) <br>
> \\(6.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp;  return \\({\mathcal K}\\) if \\(\lvert{\mathcal K}\rvert = h\\) <br>
> \\(7.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(H\leftarrow H \setminus V(T)\\)<br>
> \\(8.\\) &nbsp;&nbsp;&nbsp;&nbsp; else<br>
> \\(9.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(C\in {\mathcal K}\\) be such that \\(N_H(C)\cap \kappa_H(X) =\emptyset\\) <br>
> \\(10.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(B\leftarrow \mathrm{Reach}(N_C(H),H\setminus \{\kappa_H(X)\cup X\})\\)<br>
> \\(11.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(C\leftarrow C\cup B\\)<br>
> \\(12.\\) &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\(H\leftarrow \kappa_H(B)\\)<br>
> \\(13.\\)  &nbsp;&nbsp;&nbsp;&nbsp;   &nbsp;&nbsp;&nbsp;&nbsp; \\({\mathcal K}\leftarrow\\)<span style="font-variant: small-caps">Trim</span>\\(({\mathcal K},H)\\)<br>
> \\(14.\\) \\(S\leftarrow \bigcup_{C \in {\cal K}} \arg\min(\lvert V(C)\rvert, \lvert N_H(C)\rvert)\\)<br>
> \\(15.\\) return \\(S\\) 

***

We now show that ATS algorithm returns a small separator, or correctly certifies that \\(G\\) contains a \\(K_h\\)-minor. 

***
**Theorem 2**: For any integer \\(\ell\geq 1\\), <span style="font-variant: small-caps">ASTSeparator</span>\\((G,\ell)\\) either returns a \\(K_h\\)-minor model of \\(G\\) or a balanced separator \\(S\\) such that \\(\lvert S\rvert \leq  h^2\ell + \frac{nh}{\ell}\\).

***

Proof: Let \\(S\\) be the returned separator. We focus on bounding the size of \\(\lvert S\lvert\\); the proof for its balance is similar to Theorem 1.

We show by induction that the model \\({\mathcal K}\\) and the graph \\(H\\) satisfy the following invariant during the execution of the algorithm:

> **Invariant:** For every \\(C\in {\mathcal K}\\), either \\(\lvert V(C)\rvert\leq (h-1)\ell\\) or \\(\lvert N_H(C)\rvert\leq n/\ell\\).

The invariant clearly holds at the beginning since \\({\mathcal K} = \emptyset\\). Suppose that it holds at a current iteration in the while loop in line 2. In the next iteration, if we add a new subgraph \\(T\\) to \\({\mathcal K}\\), then \\(T\\) has size at most \\(\lvert{\mathcal K}\rvert\ell \leq (h-1)\ell\\). Otherwise, we extend a component \\(C\\) of \\({\mathcal K}\\), and we need to show that \\(\lvert N_H(C)\rvert\leq n/\ell\\) for the graph \\(H\\) in the next iteration.

Let \\(Z = H\setminus \{\kappa(H\setminus X)\cup X\}\\) be the subgraph of \\(H\\) obtained by removing \\(X\\) and the largest components \\(\kappa_H(X)\\); \\(Z\\) might be disconnected. Consider set \\(B\\) in line 10; see Figure 3. Observe that for every component \\(Y\\) of \\(Z\\) such that \\(Y\cap N_H(C)\not= \emptyset\\), every vertex of \\(Y\\) is in \\(B\\).  Thus, \\(N_H(B)\subseteq X\\) and hence \\(N_H(B\cup C)\subseteq X\\); note that this remains true even when \\(B = \emptyset\\) since in this case \\(N_H(C)\subseteq X\\).  Therefore, \\(\lvert N_H(B\cup C)\rvert\leq \rvert X\rvert\leq n/\ell\\)  by Lemma 2, implying the invariant. 

We now go back to bounding the size of the separator. The invariant means that \\(\arg\min(\lvert V(C)\rvert, \lvert N_H(C)\rvert) \leq (h-1)\ell + n/\ell\\). Since  \\(\lvert{\mathcal K}\rvert\leq h-1\\), the separator has size at most \\((h-1)((h-1)\ell + n/\ell) \leq h^2\ell + hn/\ell\\) as claimed. 

***


By choosing \\(\ell = \sqrt{n/h}\\), we obtain the following corollary:

***
**Corollary 2**: Any \\(K_h\\)-minor-free graph of \\(n\\) vertices admits a balanced separator of size \\(O(h^{3/2}\sqrt{n})\\).

***


# Referrences

[1] Alon, Noga, Paul Seymour, and Robin Thomas. "A separator theorem for nonplanar graphs." Journal of the American Mathematical Society 3, no. 4 (1990): 801-808.

[2] Plotkin, Serge A., Satish Rao, and Warren D. Smith. "Shallow Excluded Minors and Improved Graph Decompositions." In SODA 1994, pp. 462-470. 1994.
