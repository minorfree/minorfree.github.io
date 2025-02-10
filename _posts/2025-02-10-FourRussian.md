---
title:  "The Four Russian Method: Now in English"
mathjax: true
layout: post
categories: media
---

Many of us know the [Four Russians Method](https://en.wikipedia.org/wiki/Method_of_Four_Russians), a technique for speeding up Boolean matrix multiplication. The idea is pretty simple, and [Wikipedia](https://en.wikipedia.org/wiki/Method_of_Four_Russians) has a reasonably good summary, which I reproduced here:

> The main idea of the method is to partition the matrix into small square blocks of size \\(t \times t\\) for some parameter \\(t\\), and to use a lookup table to perform the algorithm quickly within each block. [.....] Thus, the overall algorithm may be performed by operating on only \\((n/t)^2\\) blocks instead of on \\(n^2\\) matrix cells, where \\(n\\) is the side length of the matrix. In order to keep the size of the lookup tables (and the time needed to initialize them) sufficiently small, \\(t\\) is typically chosen to be \\(O(\log n)\\).

I first learned the Four Russians Method from [Jeff Erickson's note](https://courses.grainger.illinois.edu/cs473/fa2024/notes/D-adv-dynprog.pdf) on dynamic programming for the edit distance.

The Four Russians Method was introduced by Arlazarov, Dini\\(\check{\mathrm{c}}\\), Kronrod, and Farad\\(\check{\mathrm{z}}\\)ev in [this paper](https://www.mathnet.ru/links/7e2c444e6a285293bd0e36a404adbf96/dan35675.pdf) about constructing the transitive closure of DAG. The paper is in Russian, and I am not aware of any English translation. 

[Ben Rozonoyer](https://brozonoyer.github.io/), a PhD student at UMass, translated the paper to English and gave me permission to publish it in my blog. Check it out [here](/assets/figs/Method_of_Four_Russians.pdf). I worked with Ben on [a paper](https://openreview.net/forum?id=HFS800reZK) about learning representation of DAGs, and that's how the four Russian paper interested him. Ben told me that only one of the four authors is ethnically Russian, while ChatGPT insists that all four are. 


