---
layout: post
title: "Sorted GCD Pair Queries"
---

Given an array, consider an sorted array of gcd(greatest common divisor) of every pairs called `gcdPairs`. For a a given query `queries[i]`, output `gcdPairs[queries[i]]`.
```cpp
class Solution{
public:
    vector<int> gcdValues(vector<int>& v, vector<long long>& queries){
        int maxx = *max_element(begin(v), end(v));
        vector<int> cntDivisors(maxx + 1, 0);
        // cntDivisors[i]: number of number have divisor i
        for(auto& x: v)
            for(int i = 1; i * i <= x; i++)
                if(x % i == 0){
                    cntDivisors[i]++;
                    if(i != x / i)
                        cntDivisors[x / i]++;
                }
        vector<long long> gcdCount(maxx + 1, 0);
        // gcdCount[i]: count pair have gcd == i
        for(int g = maxx; g >= 1; g--){
            long long count = cntDivisors[g];
            gcdCount[g] = 1LL * count * (count - 1) / 2;
            for(int mult = 2 * g; mult <= maxx; mult += g)
                gcdCount[g] -= gcdCount[mult];
        }
        /* test code
        int n = size(v);
        vector<long long> gcdCountSlow(maxx + 1, 0);
        for(int i = 0; i < n - 1; i++){
            for(int j = i + 1; j < n; j++){
                gcdCountSlow[__gcd(v[i], v[j])]++;
            }
        }
        assert(gcdCountSlow == gcdCount);
        */
        vector<long long int> pref(maxx + 1, 0);
        for(int g = 1; g <= maxx; g++)
            pref[g] = pref[g - 1] + gcdCount[g];
        vector<int> res;
        for(auto& query: queries){
            long long left = 1, right = maxx, answer = -1;
            while(left <= right){
                long long mid = (left + right) / 2;
                if(pref[mid] > query){
                    answer = mid;
                    right = mid - 1;
                } else
                    left = mid + 1;
            }
            res.push_back(answer);
        }
        return res;
    }
};
```