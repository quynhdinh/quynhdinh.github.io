---
layout: post
title: "Highly Composite Number And Number Theory"
---

A highly composite number (HCN) is a positive integer with more divisors than any smaller positive integer.

Approximately, the number of divisors for a given number is d(n) = O(n^(1/3)).

There are 38 HCNs up to 10^6.
```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAX = 1000000;
int main(){
  vector<int> num_div(MAX + 1); // num_div[i] = number of divisors of i
  for(int i = 1; i <= MAX; i++)
    for(int j = i; j <= MAX; j += i)
      num_div[j]++;
  int max_div = 0, cnt = 0;
  for(int i = 1; i <= MAX; i++)
    if(num_div[i] > max_div){
      max_div = num_div[i];
      cnt++;
      printf("%d has %d divisors\n", i, num_div[i]);
    }
  cout<<"There are "<<cnt<<" highly composite numbers up to "<<MAX<<'\n';
  return 0;
}
```

How to count how many HCNs are there up to 10^18 ?

Formula to calculate number of divisors of n = p1^e1 * p2^e2 * ... * pk^ek is (e1 + 1) * (e2 + 1) * ... * (ek + 1).