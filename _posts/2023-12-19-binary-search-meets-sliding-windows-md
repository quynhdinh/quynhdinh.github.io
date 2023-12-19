---
layout: post
title: "Binary search meets sliding windows"
---

I come across [this](https://leetcode.com/problems/apply-operations-to-maximize-frequency-score/) problem on a Leetcode contest and find it fascinating.
### [2968. Apply Operations to Maximize Frequency Score](https://www.hackerrank.com/challenges/cut-the-tree/problem)
Statement: Given an array, in one operation you can either increase or decrease any value at any position by `1`. The beauty of an arrray is the largest frequency of any nubmer. In at most `k` operation, make the array as beautiful as possible.

This is binary search on the answer. First of all, sort the input, if you are able to make array have the beauty of `K` you can make it by beauty of `K - 1`. Now the problem is given a value `X` how to check if it is possible to make an array beauty of `X` within `K` operations.
The solution to this is sliding window. For every possible window of size K, pick the middle element and make everything in the sliding window equal to this value. The cost of this is pre calculated by prefix sum.

The overall time complexity is: `O(NlogN)`
```cpp
class Solution {
public:
    int maxFrequencyScore(vector<int>& v, long long k) {
        int n = v.size();
        sort(v.begin(), v.end());
        vector<long long> preSum(n);
        preSum[0] = v[0];
        for(int i = 1; i < n; i++) {
            preSum[i] = preSum[i - 1] + (long long)v[i];
        }
        auto get_sum = [&](int i, int j) {
            return preSum[j] - (i == 0 ? 0 : preSum[i - 1]);
        };   
        auto getCost = [&](int st, int en, int mid) {
            long long costLeft = ((long long)v[mid] * (mid - st + 1)) - get_sum(st, mid);
            long long costRight = get_sum(mid, en) - ((long long)v[mid] * (en - mid + 1));
            return costLeft + costRight;
        };
        auto costEq = [&](int k) {
            long long n = v.size(), ptr1 = 0, ptr2 = k - 1, cost = LONG_LONG_MAX;
            for(; ptr2 < n; ptr2++, ptr1++) {
                if((ptr2 - ptr1 + 1) % 2 == 1) {
                    cost = min(cost, getCost(ptr1, ptr2, (ptr1 + ptr2) / 2));
                }
                else {
                    cost = min({cost, getCost(ptr1, ptr2, (ptr1 + ptr2) / 2), getCost(ptr1, ptr2, (ptr1 + ptr2) / 2 + 1)});
                }
            }
            return cost;
        };
        int L = 1, R = n, res = 1;
        while(L <= R) {
            int mid = (L + R) / 2;
            if(costEq(mid) <= k) {
                res = mid;
                L = mid + 1;
            } else {
                R = mid - 1;
            }
        }
        return res;
    }
};
```
