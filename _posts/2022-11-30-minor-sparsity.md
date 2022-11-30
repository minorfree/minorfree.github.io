---
title:  "Sparsity of minor-free graphs"
mathjax: true
layout: post
categories: media
---

Planar graphs are sparse: any planar graph with $$n$$ vertices has at most $$3n-6$$ edges. A simple corollary of this sparsity is that planar graphs are $$6$$-colorable. There is simple and beautiful proof based on the Euler formula, which can easily be exteded to bounded genus graphs, a more general case: any graph embedddable in orientable surfaces of genus $$g$$ with $$n$$ vertices has at most $$3n + 6g-6$$ edges.

How's about the number of edges of $$K_r$$-minor-free graphs? This is a very challenging question. A reasonable speculation is $$O(r)\cdot n$$: a disjoint union of $$n/(r-1)$$ copies of $$K_{r-1}$$ excludes a $$K_r$$ minor and has $$\Theta(r)\cdot n$$ edges. But this isn't the case. And surprisingly, the correct bound is $$O(r\sqrt{\log r})n$$, which will be the topic of this post.

---
**Theorem 1:** Any $$K_r$$-minor-free graphs with $$n$$ vertices has at most $$O(r\sqrt{\log r})\cdot n$$ edges. 

---

 $$ x+3+4 $$ 
 $$ e^{i\theta}=\cos(\theta)+i\sin(\theta) $$
