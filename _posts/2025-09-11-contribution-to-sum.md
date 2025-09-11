---
layout: post
title: "Contribution to sum technique"
---
This is a common technique that can be used to solve many problems. The idea is to consider the contribution of each element to the final answer instead of considering the elements themselves. Here are some problems that can be solved using this technique.

[Problem 1](https://www.hackerrank.com/challenges/sansa-and-xor/problem): Given an array. Consider all subarray of array. For each subarray, find its xor. Then find the sum of all these xors.

The naive solution is clearly not acceptable. Let takes an example, we want to find the answer for array `[1, 2, 3]`. The subarrays are [1], [2], [3], [1, 2], [2, 3], [1, 2, 3]. The answer is xor sum of all sum xors. 1 ^ 2 ^ 3 ^ (1 ^ 2) ^ (2 ^ 3) ^ (1 ^ 2 ^ 3). From here, we can see that for each number we need to count how many times it appears in the final xor sum. And we also know if we xor a number by itself even of times, it will be 0, and if it xor itself odd times, it will be the number itself. So we only need to count how many times each number appears in odd times. Now the question is for a given index. How many times subarray it is an element of. The answer is `(i + 1) * (n - i)`(1).

[Problem 2](https://leetcode.com/problems/sum-of-subarray-minimums/): Given an array of n elements. There are q queries, each query is two integers l and r. For each query, find the sum of the minimum element in each subarray of the range [l, r].

Presequisite: Monotonic stack

Again, for every number x in the array, we need to find the number of subarrays in which x is the minimum element. This can be done using a monotonic stack to find the previous less element and the next less element for each element.

Call `dp[i]`: the number of subarray ending at exactly index i where `a[i]` is the minimum element. Then we can find that `dp[i] = dp[previous_less[i]] + (i - previous_less[i]) * a[i]`, where `previous_less[i]` is the previous less element of `a[i]`. The answer will be the sum of all `dp[i]`. The answer is sum of all `dp[i]`. The `previous_less[i]` can be found using a monotonic stack in O(n) time. The overall time complexity is O(n).

[Problem 3](https://codeforces.com/contest/1165/problem/E): Given two arrays a and b of size n. Reorder the elements of array b such that the sum of `a[i] * b[i]` is minimized.

[Problem 4](https://leetcode.com/problems/sum-of-total-strength-of-wizards)

[Problem 5](https://codeforces.com/contest/617/problem/E): Given an array of n elements. There are q queries, each query is two integers l and r. For each query, find the xor sum of all elements in the range [l, r] that appear odd times in the range [l, r].

[Problem 6](https://codeforces.com/contest/617/problem/D): Given an array of n elements. There are q queries, each query is two integers l and r. For each query, find the xor sum of all elements in the range [l, r] that appear even times in the range [l, r].

(1) The number of subarrays that include the element at index i is determined by the choices of starting and ending indices for the subarray. The starting index can be any index from 0 to i (inclusive), giving (i + 1) choices. The ending index can be any index from i to n - 1 (inclusive), giving (n - i) choices. Therefore, the total number of subarrays that include the element at index i is `(i + 1) * (n - i)`