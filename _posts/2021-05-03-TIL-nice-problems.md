---
layout: post
title: "TIL - Some nice coding interview-ish I recently met"
---
I purchased Leetcode premium last week and trying to make the best out of it. Here are some problems I like.
### Course schedule III

### The number of island II
You have a grid of size `H` and `W` and they are all water cells, you have `Q` queries, that each you transform the cell `x`, `y` to land. You output the number of land for each of the query.
I naively implemented the brute force `O(H + W) * Q)` with a certain TLE then I quickly realize this as a union find problem but still didn't how to merge things on a grid. I looked the discussion tab and find a really neat trick that you flat the grid as a `H * W` length array and then you get this as the index and merge islands by the classical DSU. Overall time complexity `O(H + W).`

### 
