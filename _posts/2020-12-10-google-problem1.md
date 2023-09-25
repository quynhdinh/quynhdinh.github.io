---
layout: post
title: "Two solutions to a Google interview problems"
---
I just come aross this problem.

Given a array of balls named `colors` size of `N` each is painted the color indexed as `1, 2 or 3`. You have to process `Q` queries each have the form of `[i, c]`. Give the nearest number that have the value of `c` from the number that has the index `i` or return `-1` if the ball of color `c` doesn't exists. For example, given the array `[1, 1, 3, 3, 1](0-based).` For query `[3, 1]`, it should give `1` or `[1, 0]`, it gives `0`(the number 1 itself).

Contraints: `N, Q <= 5*10^4`

### Naive brute force solution
For each query, find the nearest number that equal the one that equal `c` from the index `i`. The complexity is `O(N*Q)`. This solution does not fit the constraints.
### Binary search solution
Build 2d-array `positions` size of 4 that `positions[i]` is the indices of balls painted the color `i`. For a query `[i, c]`, binary search to find the largest `jSmall` which is smaller than `i` and find the smallest `jBig` which is bigger than `i`. Compare and find the better one. The overall TC is `O(Q * log(N))`
### DP solution
Define `next[i][c]` is the nearest number `c` that is from the index `i`. If we can fill up this array then we can answer each query in `O(1)`. The idea is to build `next` to find the nearest from one side and then reverse the array `colors` named `next2[i][c]` which to find the nearest from the other side. For each query, return the minimum value between those two built array `min(next[i][c], next[N - i - 1][c])`.

Now, how to build `next`? Assume `next[i][c]` is to find the nearest color of `c` searching to its left and `next2` do the opposite way.
First off, `next[N-1][1] = next[N-1][2] = next[N-1][3] = INF` and `next[i][colors[i]] = 0`(The distance from the ball colored i from itself is 0). The transition is simply `next[i] = next[i + 1]`.

