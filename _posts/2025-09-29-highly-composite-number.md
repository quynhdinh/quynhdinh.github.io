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

Now to count HCN up to 10^18, we need 2 observations. An HCN X has the following properties:
1. The exponents in the prime factorization are non-decreasing
   Given a number X = p1^e1 * p2^e2 * ... * pk^ek, if there exists an i such that ei > ei+1, then we can create a smaller number Y = p1^e1 * ... * pi^(ei - 1) * pi+1^(ei+1 + 1) * ... * pk^ek which has more divisors than X. This contradicts the definition of HCN.
2. X only have prime factors up to 50
   If X has a prime factor p > 50, that means X divides 53. That means X divides the product of all primes up to 53, which is > 10^18.
3. The exponents are small. Say e1 <= 20, e2 <= 10, e3 <= 7, e4 <= 5, e5 <= 4, e6 <= 3, every else e's is <= 2
Using these observations, we can generate HCNs by starting with the smallest prime and trying all possible combinations of exponents.

Backtracking code to count HCNs up to 10^18:

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> primes = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47};
vector<int> max_exps = {20, 10, 7, 5, 4, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2};
const long long LIMIT = 1e18 + 7;
void backtrack(int idx, long long curr, int last_exp, int &cnt, const vector<int> &primes, const long long LIMIT){
  if(curr > LIMIT) return;
  cnt++;
  for(int exp = 1; exp <= last_exp; exp++){
    long long next = curr * pow(primes[idx], exp);
    if(next > LIMIT) break;
    backtrack(idx + 1, next, exp, cnt, primes, LIMIT);
  }
}
int main(){
  int cnt = 0;
  backtrack(0, 1, 1, cnt, primes, LIMIT);
  cout << "Number of highly composite numbers up to " << LIMIT << " is " << cnt << endl;
  return 0;
}
```