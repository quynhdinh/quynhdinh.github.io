---
layout: post
title: "[Tech] Stars and bars"
---

I learn this useful techniques that can solve a wide range of problems
### Stars and bars
Given `n` identical objects, The number of ways to put `n` objects into `k` boxes is `n + k - 1 choose n`.

### Direct application
Find the number of ways to satisfy the equation of `k` variables. `x_{1} + x_{2} + x_{3} + .. + x_{k} = n`.

Imagine there are `n` identicals lining up. Putting `k` bars between them. The answer is just the above equation `n + k - 1 choose n`

### `x_{i} >= 1`
Now `x_{1} >= 1`, which means there must be at most one bar between two stars. In those `n` stars lining up, there are `n - 1` gaps between those `n` stars. We want to put `k - 1` bars between them.
The solution is now `(n - 1) choose (k - 1)`
