---
layout: post
title: "Find K-th Smallest Permutation Lexicographically"
---

We are interested in the following problem: Given an array of `n` integers and an integer `k`, we want to find the `k-th` lexicographically smallest permutation of the array [1, 2, ..., n].

For example, if `n = 3` and `k = 4`, the permutations in lexicographical order are:
1. [1, 2, 3]
2. [1, 3, 2]
3. [2, 1, 3]
4. [2, 3, 1]
5. [3, 1, 2]
6. [3, 2, 1]
Thus, the 4th permutation is [2, 3, 1].

We can obviously generate all permutations and sort them, but this approach is inefficient for large `n`.

Auxiliary problem:

One school has `n` classes and number of students in each class is given by an array `a` of size `n`. We now line all students up in a single row such that all students from class 1 come first, followed by all students from class 2, and so on and all students from the same class has to be in order. Given an integer `k`, we want to find what student from which class is standing at position `k` (1-indexed) in the line.
```cpp
for(int i = 0; i < n; i++){
    if(k <= a[i]){
        cout << "Class " << i + 1 << ", Student " << k << endl;
        break;
    } else {
        k -= a[i];
    }
}
```

Now coming back to our original problem. Let's say the resulting array is `res[0], res[1], ..., res[n-1]`. We now specifically trying how to find `res[0]`. The idea is like this: we try to put number `X` at position `0` and see how many permutations can be formed with that integer at position `0`(call it `Y`). If that number is less than `k`, that means we can't afford to put `X` at position `0`, so we try the next integer and reduce `k` by `Y`. Otherwise, we can put `X` at position `0` and move on to position `1`.
```cpp
for(int v = 1; v <= n; v++) {
    int cnt = ???; // number of permutations with v at position 0
    if(cnt < k) {
        k -= cnt;
    } else {
        res[0] = v;
        break;
    }
}
```
We can now conclude that the `res[0]` has to be `v` and also `res` has to be the `k-th`(which might be subtracted) permutation of permutation that starts with `v`.

Now, how to find `res[1]` ?

We can apply the same logic. We try to put number `X` at position `1`(but this X can't be `res[0]`), and see how many permutations can be formed with that integer at position `1`. If that number is less than `k`, we try the next integer and reduce `k` by that number. Otherwise, we can put `X` at position `1` and move on to position `2`.

```cpp
for(int v = 1; v <= n; v++) {
    if(v is already used) continue;
    int cnt = (n - 1)!; // number of permutations with v at position 1
    if(cnt < k) {
        k -= cnt;
    } else {
        res[1] = v;
        break;
    }
}
```

We can repeat this process until we fill all positions in the result array.

In practice, to iterate through the unused numbers efficiently, we can just maintain the unused numbers in a data structure like a set.

Here is the complete C++ code to find the `k-th` lexicographically smallest permutation of the array [1, 2, ..., n]:

```cpp
#include <iostream>
#include <vector>
#include <set>
using namespace std;
int main(){
    int n, k;
    cin >> n >> k;
    vector<long long> fact(n + 1, 1);
    for(int i = 1; i <= n; i++) {
        fact[i] = fact[i - 1] * i;
    }
    set<int> unused;
    for(int i = 1; i <= n; i++) {
        unused.insert(i);
    }
    vector<int> res(n);
    for(int i = 0; i < n; i++) {
        for(int v : unused) {
            long long cnt = fact[n - i - 1];
            if(cnt < k) {
                k -= cnt;
            } else {
                res[i] = v;
                unused.erase(v);
                break;
            }
        }
    }
    for(int v : res)
        cout << v << " ";
    cout << endl;
    return 0;
}
```