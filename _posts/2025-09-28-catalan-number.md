---
layout: post
title: "Catalan Number"
---

Catalan numbers are a sequence of natural numbers that occur in various counting problems in combinatorial mathematics. The n-th Catalan number can be expressed directly in terms of binomial coefficients by the formula:
```
C(n) = (1 / (n + 1)) * (2n choose n)
     = (2n)! / ((n + 1)! * n!)
```
The first few Catalan numbers for n = 0, 1, 2, ... are:
0, 1, 1, 2, 5, 14, 42
Catalan numbers have many applications in combinatorial problems, including:
- Counting the number of valid parentheses expressions
- Counting the number of rooted binary trees with n+1 leaves
- Counting the number of ways to connect n points on a circle with non-crossing chords
- Counting the number of ways to triangulate a polygon with n+2 sides
- Counting the number of monotonic lattice paths along the edges of a grid with n x n squares that do not pass above the main diagonal
- Counting the number of ways to arrange n pairs of parentheses such that they are correctly matched
- Counting the number of stack-sortable permutations of n elements
- Counting the number of non-isomorphic full binary trees with n internal nodes