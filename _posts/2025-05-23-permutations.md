---
layout: post
title: "Permutations"
---

A sorted permutation has `n` permutation each has size `1`

A permutation with 1 version has `n - 2` cycles of size `1` and 1 cycle of size `2`

If you swap two number in the same cycle you will split that cycle into 2 cycles and if you merge 
2 number in 2 different cycle you will merge two cycle into one big cycles.

If `K` is the number of cycles in a permutation of size `N`. Then the number of way to sort a permutation is `N - K`.

