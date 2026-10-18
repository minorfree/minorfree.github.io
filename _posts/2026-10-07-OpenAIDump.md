---
title:  "OpenAI Math Dump Hits My Home"
mathjax: true
layout: post
categories: media
date: 2026-10-07 14:35:00 -0400
---

Recent OpenAI [Math dump](https://github.com/openai/math/tree/main/preprints) have solutions to many long standing probems in Math and TCS. In the dump, two problems that I and my friends, notably [Arnold Filtser](https://arnold.filtser.com/), have studied for more than a decade, and published a few papers about this:

1. The $\ell_1$-embedding conjecture for planar graphs. Open AI solution [here](https://github.com/openai/math/blob/main/preprints/Planar-Graph-Metrics-Embed-into-L1-with-Constant-Distortion-September-23-2026/paper.pdf).
2. The $\ell_1$-embedding conjecture for bounded treewidth graphs.  Open AI solution [here](https://github.com/openai/math/blob/main/preprints/L1-Embeddings-of-Graphs-of-Bounded-Treewidth-September-23-2026/paper.pdf).


It is unsettling and hard to swallow. I have not looked at the details yet, and will be doing so in the next few days. On a positive note, I hope to learn new techniques in planar graphs. I always believe that we have not been able to solve these problems because we lack a serious  understanding of planar metrics. Now that they are solved, learning what the serious understanding is exciting.

A clear next prediction (not included among Open AI solution) is a solution of the conjecture that that minor-free graph metrics are embeddable into $\ell_1$ with constant distortion. Using the Robertson-Seymour decomposition, one basically could reduce this conjecture to  bounded treewidth and planar graphs.    At this point, I feel that understanding the two results above are more important than churning out another result. 

Will udpate my understsanding of the two papers above.

I intentially do not mention other big results. What a strange time to be alive.

Updates:

- Oct 18: Spent 5 hours today: 2 hours consolidating my understanding of Section 7 and 3 hours reading Section 8. It is strange that, to understand the text at the beginning of Section 8, I have to read Section 8. It’s like the (pointless) proof overviews of typical AI papers: to understand the overview, you have to read the actual proof. Section 8 is super dense. I managed to go through Section 8.1 only, and will read 8.2 tomorrow in more details. I got a very hazy understanding of Section 8.1.

- Oct 17: Read Section 7 for 4 hours and got a decent understanding of it. It introduces the concepts of an ordered field, which will be mainly used in Section 8. The big picture of how the ordered fields will be used is still unclear at this moment, but will be clear when I read Section 8 next.  All proofs in this section are relatively simple. The biggest challenge is to deal with the notation and keep all the crazy constants in my head.


- Oct 14-16: continue reading for 5.5 hours over three (busy) days. I finished reading Section 6 and have a good understanding of it. Reading through this section makes me appreciate the column model (introduced in Section 2) much more. Using the column model, the key argument (the packing bound) is quite slick. One annoyance I have is that often in computer science, we draw tree upside down (yes, CS is weird), but in this proof, I have to picture the shortest path tree upward, which confuses me greatly when it comes to terminology likes descending path. However, the reading starts to get more interesting as now I can understand the big picture better. And I am looking forward to reading Section 7 in the next few days.

- Oct 12-13: continue reading for 6.5 hours over two days. I have not finished understanding the whole Section 6, but I now get a decent understanding of Sections 6.1 and 6.2. I noticed that some arguments in the proof of these sections are very familiar to me. But a large part of this section is new. I am still wrestling with the notation. On a positive note, after Section 5, the paper gets increasingly interesting, and I feel like I am learning something new.  Still, I have not gotten to the center of it. (No more complain about the writing from now on since it is bad by default.)

- Oct 11: continue reading for 3 hours. I spent 2 hours reviewing Section 5 again and got a better understanding of this section: it sets the stage for the real actions, which are in Sections 6, 7, and 8. I also started reading Section 6.1 for an hour. The writing of Sections 5 and 6 is a complete joke. (Why am I even saying this? I should just assume this by default.) 

- Oct 10: Continue reading for 3 hours. The interpolation of Section 4 (specifically Corollary 4.6) seems like a very nice trick. (I still don't know how it will be used yet.) So I decided to spend 2 more hours trying to understand the statement of Corollary 4.6, with all the subtleties of the randomness involved. Moving to section 5, it describes a very vague idea of the scales. Scaling is a standard trick, but it is likely that this paper uses it in a subtle way that I am not aware of. The details are not in Section 5, which is where I stop for now. 

- Oct 09: Continue reading for 3.5 hours (don't have a lot of time today). I spent 2.5 hours consolidating my understanding of the chart, and I think I got a good handle on this object. The reading confirms my impression from yesterday: the objects are very abstract and hard to understand, but the lemmas in Section 3 are simply reformulations of shallow facts about these abstract objects (which make them sound deep and terrifying). I spent 1 hour reading the statement of Theorem 4.1 (and yes, another hour just to understand what it means). To keep my sanity, I will skip the proof (which is the entire Section 4), and jump to the next section (Section 5) tomorrow (if I have time to read tomorrow at all).  

- Oct 08: Read the L1 embedding planar for 5 hours. Now that I understand the column model better, the lemmas in the first 12 pages are straightforward. I spent a lot of time figuring out what the statements of these lemmas actually mean. The proofs are straightforward, and nothing deep happens yet. I also spent about 3 hours reading section 3 of the manuscript. It is only 4 pages long, but full of gibberish. The central concept in this section is called a **chart**. Nothing in this section is formal: I have to interact with GPT Anstra just to understand what each paragraph means. So far, I get the impression that nothing deep happens in this section either. Perhaps great stuff will be in subsequent sections, so I am looking forward to that. It has been a lot of pain going through Sections 2 and 3. 

- Oct 07: Read planar L1 Embedding manuscript of Open AI for about 6 hours, gone through the details of the first 12 pages. The proof introduces a very strange model called the **non-crossing column model** of planar graphs, that I have not seen before. I have not yet internalized the model but got a "feel" for it. The writing is so compressed and dense. The expanded version (obtained by querying ChatGPT Astra) has 90+ pages. The "length" of the manuscript released by Open AI is cheated. 