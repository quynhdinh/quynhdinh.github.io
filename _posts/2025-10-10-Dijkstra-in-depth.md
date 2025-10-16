---
layout: post
title: "Dijkstra in depth(WIP)"
---

For the long time I think the Dijkstra algorithm only has a usage in finding the shortest path in a graph.

Consider the graph `G = (V, E)`. `S` is the non-empty source set that S ⊆ V. 
A path `p` is a vector of vertices `{v1, v2, .., vk}` and the function `f` maps paths to real number. If `f` satisfies these 3 following properties, then Dijkstra algorithm can be used to find the optimal solution.
1. induction base. For all s ∈ S, f(s) should be correctly initialized.
2. Extension property. For all p, for every vertex v adjacent to the end of p (p.back()), f(p U v) >= f(p)
3. Dynamic programming property(dp) property. If p, q are two paths that end at the same vertex, and f(p) <= f(q), then for every vertex v adjacent to the end of p and q, f(p U v) <= f(q U v)