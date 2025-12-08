---
title:  "A Separator for Minor-free Graphs in Linear Time"
mathjax: true
layout: post
categories: media
---

I am stoked to write this post, about [our linear-time algorithm](https://arxiv.org/abs/2512.01587) for finding a balanced separator of minor-free graphs, joint with amazing collaborators [Édouard Bonnet](https://perso.ens-lyon.fr/edouard.bonnet/), [Tuukka Korhonen](https://tuukkakorhonen.com/), [Jason Li](https://q3r.github.io/), and [Tomáš Masařík](http://tarken.krakonos.org/). Special thanks to the [AlgUW workshop](https://workshop.mimuw.edu.pl/) (Bedlewo 09.2025) for giving us an opportunity to get together and solve this problem!


I wrote about [separator theorems for minor-free graphs](https://minorfree.github.io/sep-theorems/) in the past. (Interestingly, Tuukka also left a comment on the post back then, pointing to his [separator theorem for induced-minor-free graphs](https://arxiv.org/abs/2308.04795); now we finally had a chance to work together with others on the separator theorem.) One key issue swept under the rug is that none of the algorithms is known to have a linear time implementation.  The [Lipton-Tarjan separator theorem](https://en.wikipedia.org/wiki/Planar_separator_theorem), on the other hand, can be easily implemented in linear time. I consider this a big gap between planar and minor-free graphs, and closing this gap has been a problem that I have kept coming back to once in a while. 

The [seminal result of  Alon, Seymour, and Thomas](https://www.jstor.org/stable/1990903) in 1990 gave a separator of size \\(O(\sqrt{n})\\) in time \\(O(n^{3/2})\\). There have been a few other papers since then trying to reduce the running time to \\(O(n)\\). A one-sentence summary is that: either the separator has a larger size (\\(n^{2/3}\\)), the running time is non-linear (\\(\approx n^{4/5}\\), or the algorithm is so complicated (to the point where the full version is still nowhere to be found). See our paper for a more detailed discussion. 

Now we have a simple algorithm to find a separator of size \\(O(\sqrt{n})\\) in time \\(O(n)\\). (The dependency on the minor size of our algorithm is polynomial, and will not be spelled out in this post.) Without further ado, here it is. 

 
# The Algorithm

The following algorithm finds a \\((1-\frac{1}{200h^2})\\)-balanced separator \\(S\\): every connected component of \\(G-S\\) has size at most \\((1-\frac{1}{200h^2})n\\). Here \\(h\\) is the size of the excluded minor: \\(G\\) is \\(K_h\\)-minor-free. One can get a \\(2/3\\)-balanced separator by repeating this algorithm \\(O(h^2)\\) times. Notice the strange numbers \\(200, 40, 20, 20^3\\). These are chosen to make the analysis simpler; we make no attempt to optimize them. 

***
 <span style="font-variant: small-caps">FindSeparator</span>\\((G = (V,E))\\)
> 1. \\(w_1(v)\leftarrow 40\\) for every \\(v\in V\\)
> 2. \\(S\leftarrow \emptyset\\)
> 3. for \\(t\leftarrow 1\\) to \\(20h^2\\) 
> 4. &nbsp;&nbsp;&nbsp;&nbsp; <mark>\\((C^{*}_t, S_t)\leftarrow \text{KPR}(\langle G,w_t\rangle,\lfloor\sqrt{n}/6h^2\rfloor)\\)  </mark> 
> 5. &nbsp;&nbsp;&nbsp;&nbsp; \\(S\leftarrow S\cup S_t\\)
> 6. &nbsp;&nbsp;&nbsp;&nbsp; if \\(\rvert C^{\ast}_t\rvert \leq (1-\frac{1}{200h^2})n\\)
> 7. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; return \\(S\\)  
> 8. &nbsp;&nbsp;&nbsp;&nbsp; \\(c_t\leftarrow\\) an arbitrary vertex in \\(C^{\ast}_t\\)
> 9. &nbsp;&nbsp;&nbsp;&nbsp; <mark>\\(T_{t}\leftarrow\text{BFS}(\langle{G,w_t}\rangle,c_t,\sqrt{n})\\)</mark> 
> 10. &nbsp;&nbsp;&nbsp;&nbsp; for every \\(v\in V(T_{t})\\) 
> 11. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; \\(w_{t+1}(v)\leftarrow (1+ \frac{\lvert T_{t}(v)\rvert 20^3h^6}{\sqrt{n}})w_t(v)\\)
> 12. &nbsp;&nbsp;&nbsp;&nbsp; return \\(S\\)
***

In line 11, \\(T_t(v)\\) is the vertex set of the  subtree rooted at \\(v\\). Three things that I'd like to clarify:

1. Each vertex \\(v\\) will be given an integer weight \\(w_t(v)\\) in each iteration, which plays a crucial role. We denote by \\(\langle{G,w_t}\rangle\\) the graph \\(G\\) with weight function \\(w_t\\) on the vertices. Throughout the algorithm, we will guarantee that the total weight remains small: \\(\sum_{v\in V}w_t(v) = O(n)\\). (The weight update in line 11, as is written, could be fractional; we can fix this by rounding up to the nearest integer.) The role of the weight function will be clarified in the ananlysis below.

2. \\(\text{BFS}(\langle{G,w_t}\rangle,c_t,\sqrt{n})\\) is the vertex-weighted shortest path tree rooted at vertex \\(c_t\\) and truncated at radius \\(\sqrt{n}\\).  This tree can be found by doing vertex-weight BFS in \\(O(n)\\) time.

3. \\(\text{KPR}(\langle G,w_t\rangle,\lfloor\sqrt{n}/6h^2\rfloor,h)\\) is the vertex-weighted variant of the **KPR decomposition**, named after [Klein, Plotkin, and Rao](https://dl.acm.org/doi/abs/10.1145/167088.167261), with parameter \\(\Delta = \lfloor\sqrt{n}/6h^2\rfloor\\). The KPR decomposition, given an unweighted \\(K_h\\)-minor-free graph \\(G\\) and a parameter \\(\Delta > 0\\), decomposes \\(G\\) into connected components of weak (hop) diameter \\(O(\Delta)\\) by removing \\(O(n/\Delta)\\) edges. When \\(\Delta \approx \sqrt{n}\\), the number of edges removed is \\(O(\sqrt{n})\\), coinciding with the separator bound that we are aiming for. The decomposition is obtained by computing BFS trees in \\(h\\) rounds. The vertex-weighted variant works the same way, summarized in Lemma 1 below.

Overall, our algorithm finds the separator by doing (vertex-weight) BFS \\(O(h^3)\\) time.


***
**Lemma 1:** Given a graph \\(\langle G = (V,E),w\rangle\\) be a \\(K_h\\)-minor-free graph with integer weights on vertices, and integer paramete \\(\Delta > 0\\). Let \\(W = \sum_{v\in V}w(v)\\). The procedure \\(\text{KPR}(\langle G,w\rangle, \Delta)\\) runs in  time \\(O(\text{poly}(h)(n+W))\\) and returns  a pair \\((C^{\ast}, S)\\) where \\(S\\) is a subset of vertices of size \\(h\cdot W/\Delta\\), and \\(C^{\ast}\\) is the connected component of maximum size of  \\(G - S\\), which is guaranteed to have (vertex-weighted) weak diameter at most \\(6\cdot h^2\Delta\\).

***


As for the case of KPR, it is the best to present the algorithm as a randomized one, which could be easily derandomized; see the appendix [our paper](https://arxiv.org/abs/2512.01587) for details.



***
 <span style="font-variant: small-caps">KPR</span>\\((G = (V,E),w,\Delta)\\)
> 1. \\({\mathcal C}_1\leftarrow \langle G,w\rangle\\) and \\(i\leftarrow 1\\)
> 2. \\(S\leftarrow \emptyset\\)
> 3. while \\(i\leq h\\)
> 4. &nbsp;&nbsp;&nbsp;&nbsp; choose random \\(\tau \in [0,1]\\)
> 5. &nbsp;&nbsp;&nbsp;&nbsp; for each \\(C\in {\mathcal C}_i\\)
> 6. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; pick a vertex \\(r\in C\\)
> 7. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; \\(S\leftarrow S\cup \{v\in C: d_C(r,v) \equiv \tau \Delta ~(\text{mod}~ \Delta)\}\\)
> 8. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; add all connected components of \\(C-S\\) to \\({\mathcal C}_{i+1}\\).
> 9. &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;  \\(i\leftarrow i+1\\)
> 10. \\(C^{\ast}\\) largest component in \\({\mathcal C}_{h+1}\\)
> 11. return \\((C^{\ast},S)\\)
***

Basiscally, we takes all the layers of the form \\(j\Delta + \tau\Delta\\) for all integers \\(j\\). (W.l.o.g. we assume \\(\tau\Delta\\) is an integer by a simple rouding).  Line 7 requires computing a vertex-weighted BFS tree on \\(C\\) with the (integer) weights are inherited from \\(G\\). The analyis is along the line of KPR: the basic idea is that in each iteration \\(i\\), the probability that \\(v\in S\\) is exaclty \\(w(v)/\Delta\\) (due to the random choice \\(\tau\\)). Thus, the expected total size of \\(S\\) after \\(h\\) iterations is \\(hW/\Delta\\). Bounding the weak diameter of \\(C^{\ast}\\) is the same as the original KPR; see see [here](https://tcsmath.wordpress.com/2012/01/11/a-simpler-proof-of-the-kpr-theorem/) for a simple proof.

# The Analysis

**The separator size.**  Bounding the separator's size boils down to bounding the total weight of every vertex at each iteration. 

***
**Lemma 2:** Let \\(W_t = \sum_{v}w_t(v)\\), then \\(W_t = O(h^{8} n)\\). 

***

Assuming Lemma 2, then by Lemma 1, each iteration, we add \\(O(h\cdot h^{8}n h^2/\sqrt{n}) = O(h^{11} \sqrt{n})\\) vertices to \\(S\\). Thus, over \\(O(h^2)\\) iterations, \\(S\\) has size \\(O(h^{13}\sqrt{n}) = O(\sqrt{n})\\).



**Correctness.** We need to show that \\(S\\) is a \\((1-\frac{1}{200h^2})\\)-balanced separator. Indeed, we will be showing something stronger: line 12 in the algorithm will never be reached, and at some point, the algorithm invokes line 7 and terminates. The vertex weighing scheme is central to everything.

Here is the rough intuition:  Initially, every vertex \\(v\\) has the same role and hence is given the same weight \\(w_1(v) = 40\\). When we apply KPR to find a separator \\(S_t\\) (at an iteration \\(t\\)), the largest component \\(C^{\ast}_t\\) of \\(G-S_{t}\\) is only guaranteed to have a small diameter (of \\(O(\sqrt{n})\\)); it could contain almost all the vertices. If in the next iteration, we simply apply KPR again on the large component, we might not get anything, i.e., the returned separator is empty. By increasing the current weight of a vertex  \\(v\\)

$$w_{t+1}(v)\leftarrow  w_t(v)\left(1 + \frac{\lvert T_{t}(v)\rvert 20^3h^6}{\sqrt{n}}\right),$$

the (vertex-weighted) diameter of the component in the next iteration increases. (In the algorithm, we apply BFS to \\(G\\) in every iteration, but we should think of this as applying BFS to the largest component from the previous iteration.)  Vertices that have many descendants in \\(T_t\\) have more weights, and therefore, are more likely to be added to the separator in the next iteration.

After \\(20h^2\\) iterations, we could construct a \\(K_h\\)-minor of \\(G\\) from a collection of trees  \\({\mathcal T} = \{T_1,T_2,\ldots, T_{20h^2}\}\\) produced by the algorithm. Precisely how will be discussed later. Here, I would like to point out that the construction of the minor gives rise to the exact weighing scheme above. The upshot:  if \\(G\\) is \\(K_h\\)-minor-free, the algorithm has to terminate before  \\(20h^2\\) iterations. 

Let \\({\mathcal R}(T_t)\\)  be the distribution of rooted paths of \\(T_t\\) constructed as follows: pick a vertex \\(u\\) in \\(T_t\\) uniformly at random, and return the path from \\(u\\) to the root of \\(T_t\\).  Let \\(E_{t,v}\\) be the event that a random path  \\(P\sim {\mathcal R}(T_t)\\) contains \\(v\\). \\(T_t\\) is the tree at iteration \\(t\\).). The algorithm maintains the following five invariants. The most interesting invariants are 1,2 and 3, and invariant 3 implies Lemma 2.

***
**Lemma 3:** Let \\(k = 20h^2\\). The following invariants are maintained at every iteration \\(t\\):
1. \\(\mathrm{Pr}[E_{i,v}]\leq (20k^3\,\sqrt{n})^{-1}\,w_{t+1}(v)\\) for every \\(i \leq t\\) and every \\(v\\).
2. \\(\mathrm{Pr}[E_{i,v}\cap E_{j,v}] \leq (10k^6\,n)^{-1}\,w_{t+1}(v)\\) for every \\(i\not= j \leq t\\) and every \\(v\\).
3. \\(\sum_{v\in V}w_t(v)\leq (40 + (k^3 + 1)(t-1))n\\).
4. \\(\rvert V(T_t)\rvert \geq (1-\frac{1}{10k})n\\).
5. \\(w_t(v)\geq 40\\) for every \\(v\\).

***

**Proof Sketch.** Invariants 4 and 5 follow directly from the definition. Invariant 3 follows from induction and the definition of \\(w_{t}(v)\\). For invariant 1, observe that \\(\\)\mathrm{Pr}[E_{i,v}] = \frac{\lvert T_{t}(v)\rvert }{\lvert V(T_t)\rvert }  \leq  \frac{2\rvert T_{t}(v)\rvert }{n},\\)\\) as \\(V(T_t)\geq n/2\\). The definition of \\(w_{t+1}(v)\\) gives:

$$\lvert T_t(v)\rvert \leq \frac{\sqrt{n}\,w_{t+1}(v)}{k^3\,w_t(v)}$$

Invariant 1 then follows from \\(w_t(v)\geq 40\\) and simple calculations. Invariant 2 follows from the following observation (and simple calculations):

$$\mathrm{Pr}[E_{i,v}\cap E_{t,v}] = \mathrm{Pr}[E_{i,v}] \mathrm{Pr}[E_{t,v}]  \leq \frac{w_t(v)}{20 k^3\sqrt{n}}\frac{\lvert T_{t}(v)\rvert }{\lvert V(T_{t})\rvert } $$ 
 
***

The invariants are pretty boring, but the consequence we are looking for is this:

***
**Lemma 4:**  Suppose that the algorithm does not return a separator \\(S\\) after \\(k\\) iterations, then \\({\mathcal T} = \{T_1,\ldots, T_k\}\\) has the following properties:

1. \\(\rvert V(T_i)\rvert \geq n-n/(10k)\\) for every \\(i\in [k]\\).
2. For every \\(i,j\in [k]\\) such that \\(i\not= j\\), \\(\Pr_{P\sim {\mathcal R}(T_i), Q\sim {\mathcal R}(T_j)}[P\cap Q \not=\emptyset] \leq \frac{1}{5k^2}\\).

***
 
We call \\({\mathcal T}\\) a **stochastic connectior**, for the following reason.


 
***
**Lemma 5:**  Given a stochastic connector \\({\mathcal T}\\) of size \\(k\\) in a connected graph \\(G\\) with \\(m\\) edges, for any \\(t\\) such that \\(20t^2\leq k\\), we can construct a  \\(K_t\\)-minor model of \\(G\\) in \\(O(km)\\) time with probability at least \\(2/5\\).

***

**Proof Sketch.** By a simple reduction, it suffices to construct a minor \\(H\\) which is subcubic and \\(\rvert V(H)\rvert + \lvert E(H)\rvert \leq 20t^2\\). The minor is represented by a random mapping \\(\phi\\) mapping each vertex and edge to \\(G\\), called an *almost-embedding*.

1. For every vertex \\(v\in V(H)\\), we assign \\(\phi(v)\\) to be a vertex in \\(V(G)\\) chosen u.a.r.
2. For each edge  \\(e = uv \in  E(H)\\),  we associate \\(e\\) with a distinct tree in \\({\mathcal T}\\), denoted by \\(T_e\\). This is possible since \\(k\geq \lvert E(H)\rvert\\). Then we assign \\(\phi(e)\\) to be the path \\(T_e[u,v]\\) in \\(T_e\\). (It could be that \\(u\\) or \\(v\\) is not in \\(T_e\\),  and in this case, we set \\(\phi(e) = \emptyset\\).)

Basically, each tree in \\({\mathcal T}\\) connects the endpoints of each edge of \\(H\\). 

To construct a \\(K_t\\)-minor model from the embedding \\(\phi\\), we need the property that: for every two edges \\(e_1,e_2\\) in \\(H\\) that do not share an endpoint, \\(\phi(e_1)\cap \phi(e_2) = \emptyset\\). The probability of this happening, using the union bound, is at least \\(2/5\\).

***
 
If you found all of these lemmas too sketchy, check out [our paper](https://arxiv.org/abs/2512.01587) for details.  

The dependency on \\(h\\) in our algorithm is still rather large, so an open problem is:

***
**Open Problem**:  Design an algorithm with running time \\(\tilde{O}(h\,n)\\) producing a separator of size \\(\tilde{O}(h\sqrt{n})\\).

***

The \\(\tilde{O}\\) hides a \\(\mathrm{polylog}(h)\\) factor.
