---
layout: post
title: "Double Prefix Sum Technique"
---
Prefix sum is a common technique to calculate the sum of elements in a range efficiently.
Double prefix sum is an extension of this technique that can be used to solve more complex problems. Just like you can expected, we will calculate prefix sum of prefix sum of a given array. But why would we need to do that?

Consider this problem: Given an array of `N` elements. There are `Q` queries, each query is given 3 integers `l`, `r`, and `i` you are asked to find the sum of subarray sums of all subarrays in the range `[l, r]` that include the element at index `i`.

The naive solution is to iterate through all subarrays in the range `[l, r]` could be and check if they include the element at index `i`(it could be up to `N * (N + 1) / 2` subarrays). If they do, we add their sum to the answer by using a prefix sum. This approach is clearly not efficient and will not work for large inputs.

To solve this problem efficiently, we can use the double prefix sum technique. First, we need to understand how many subarrays in the range `[l, r]` include the element at index `i`. The answer is `(i - l + 1) * (r - i + 1)`. This is because the starting index of the subarray can be any index from `l` to `i`, and the ending index can be any index from `i` to `r`.

Now consider an array `A` of size 10. `a1`, `a2`, `a3`, `a4`, `a5`, `a6`, `a7`, `a8`, `a9`, `a10`. And we need to calculate the sum of all subarrays in the range `[2, 7]` that include the element at index `5`. Here are all subarray that inside the range `[2, 7]` that include the element at index `5`:
```
    a1 [a2]  a3  a4 [a5] a6 [a7] a8  a9  a10
        |---------------|
        |------------------|
        |----------------------|
             |----------|
             |-------------|
             |-----------------|
                 |------|
                 |---------|
                 |-------------|
                    |---|
                    |------|
                    |----------|
```
Now call P as the prefix sum array of A. `P[i]` is the sum of the first `i` elements of `A` that is `P[i] = P[i - 1] + A[i]`
Consider the sum of all subarray starting from `a2`(There are 4 of them)
`p2_1 = P[5] - P[1]`

`p2_2 = P[6] - P[1]`

`p2_3 = P[7] - P[1]`

Now the sum of all subarray starting from `a3`

`p3_1 = P[5] - P[2]`

`p3_2 = P[6] - P[2]`

`p3_3 = P[7] - P[2]`

Now the sum of all subarray starting from `a4`

`p4_1 = P[5] - P[3]`

`p4_2 = P[6] - P[3]`

`p4_3 = P[7] - P[3]`

We now can see the right parts of those differece are the same and they exists `3` times each (or `r - i + 1` times)

We now can see the left parts of those differece are the same and they exists `4` times each (or `i - l + 1` times)

Call ``PP`` as the prefix sum array of P. That is `PP[i]` is the sum of the first `i` elements of `P`. ``PP[i] = PP[i - 1] + P[i]``

The sum of all subarrays in the range `[l, r]` that include the element at index `i` can be calculated as follows:

```sum = (i - l + 1) * (PP[r] - PP[i - 1]) + (r - i + 1) * (PP[i] - PP[l - 1])```