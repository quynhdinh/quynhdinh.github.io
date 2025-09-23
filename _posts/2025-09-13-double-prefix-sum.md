---
layout: post
title: "Double Prefix Sum Technique"
---
Prefix sum is a common technique to calculate the sum of elements in a range efficiently.
Double prefix sum is an extension of this technique that can be used to solve more complex problems. Just like you can expected, we will calculate prefix sum of prefix sum of a given array. But why would we need to do that?

Consider this problem: Given an array of `N` elements. There are `Q` queries, each query is given 3 integers `l`, `r`, and `i` you are asked to find the sum of subarray sums of all subarrays in the range `[l, r]` that include the element at index `i`.

The naive solution is to iterate through all subarrays in the range `[l, r]` could be and check if they include the element at index `i`. If they do, we add their sum to the answer by using a prefix sum. This approach is clearly not efficient and will not work for large inputs.

Now consider an array `A` of size 10. `a1`, `a2`, `a3`, `a4`, `a5`, `a6`, `a7`, `a8`, `a9`, `a10`. And we need to calculate the sum of all subarrays in the range `[l, r] = [2, 7]` that include the element at index `5`. Here are all subarray that inside the range `[l, r] = [2, 7]` that include the element at index `5`:
```
    a1 [a2]  a3  a4 [a5] a6 [a7] a8  a9  a10
       |----------------| <- p2_1
       |-------------------| <- p2_2
       |------------------------| <- p2_3
             |----------| <- p3_1
             |-------------| <- p3_2
             |-----------------| <- p3_3
                 |------| <- p4_1
                 |---------| <- p4_2
                 |--------------| <- p4_3
                    |---| <- p5_1
                    |------| <- p5_2
                    |-----------| <- p5_3
```
Now call `P` as the prefix sum array of A. `P[i]` is the sum of the first `i` elements of `A` that is `P[i] = P[i - 1] + A[i]`
Consider the sum of all subarray starting from `a2`(There are 3 of them)

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

Now the sum of all subarray starting from `a5`

`p5_1 = P[5] - P[4]`

`p5_2 = P[6] - P[4]`

`p5_3 = P[7] - P[4]`

We now can see the right parts of those differences are the same and they exists `3` times each (or `r - i + 1` times). Each of those elements are `P[5]`, `P[6]`, and `P[7]` or `P[i]`, `P[i + 1]`, ..., `P[r]`

Also left parts of those differences are the same and they exists `4` times each (or `i - l + 1` times). Each of those elements are `P[1]`, `P[2]`, `P[3]`, and `P[4]` or `P[l - 1]`, `P[l]`, ..., `P[i - 1]`

We now need a way to calculate the sum of subarray of `P` efficiently. Of course, we can apply prefix sum again on the prefix sum `P`. Here come the name double prefix sum.

Call ``PP`` as the prefix sum array of `P`. That is `PP[i]` is the sum of the first `i` elements of `P`. ``PP[i] = PP[i - 1] + P[i]``

Put them all together, the sum of all subarrays in the range `[l, r]` that include the element at index `i` can be calculated as follows:

```sum = (i - l + 1) * (PP[r] - PP[i - 1]) - (r - i + 1) * (PP[i - 1] - PP[l - 2])```

The preprocessing time is O(N) and the answer for each query is O(1)

Sometimes, something that looks complex can be solved by writing things down and looking for patterns. Double prefix sum might help you to have an idea when asked to calculate the sum of sums in a range with some constraints.

One quite hard problem that use this technique as a step is [LC-2281](https://leetcode.com/problems/sum-of-total-strength-of-wizards/)

That's it for now. See you in the next post.