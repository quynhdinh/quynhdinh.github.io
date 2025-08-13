---
layout: post
title: "Avoiding Overflow: Using Logarithms to Transform Product Queries"
---
Ever wonder what logarithms are good for other than just a math concept? Here is a neat trick that utilizes logarithms that might come in handy in competitive programming or algorithm design.

Consider this problem: Given an array of integers of size N, where a number could be up to `10^9`, then you have `Q` queries, each query is a range `[left, right]`, you need to find the product of all numbers in that range. If the product is larger than `10^9`, you can just return `10^9`.

The naive solution is for each query, you iterate through the range and calculate the product. This will take `O(N * Q)` time, which is not efficient for large inputs.

In languages that support big integers like Java or Python, you can use a big integer type to store the product to avoid overflow. However, in languages like C++ or Go, you might run into overflow issues with standard integer types.

One cool trick is to replace each number with its logarithm. Instead of calculating the product, you can calculate the sum of logarithms. And you can use prefix sum to calculate that. Why does this work? Because of the property of logarithms:

log(a * b) = log(a) + log(b)

Note: In this trick, bases are not really important. The base of logrithm can be any positive number except `1`. Here, we use the natural logarithm (base `e`).

And if you want to retrieve the product from the sum of logarithms, you can use the exponential function on that sum. Because:

a * b = e ^ log(a * b)

The illustration code is as follows:
```cpp
const int MAX_VAL = 1e9;
vector<int> arr = {MAX_VAL, MAX_VAL, MAX_VAL};
vector<double> logs(arr.size());
for(int i = 0; i < arr.size(); i++) {
    logs[i] = log(arr[i]);
}
vector<double> prefix_sum(arr.size());
prefix_sum[0] = logs[0];
for(int i = 0; i < arr.size(); i++) {
    prefix_sum[i] = prefix_sum[i - 1] + logs[i];
}
auto query = [&](int l, int r) {
    double sum = prefix_sum[r] - (l > 0 ? prefix_sum[l - 1] : 0);
    if (sum > log(1e9)) {
        return 1e9;
    }
    return exp(sum);
};
```
By doing this, if all number of the array are maximum `10^9` whose log is ~20, the maximum sum of logarithms is just `N * 20`. So you can easily store the prefix sum in a `double` array without worrying about overflow.

There you go, you can now answer each query in `O(1)` time after `O(N)` preprocessing time. This is a neat trick that can be applied to various problems involving products, especially when the numbers involved are large and can lead to overflow issues.