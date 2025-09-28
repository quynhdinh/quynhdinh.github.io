---
layout: post
title: "The block-of-64 trick"
---

Consider this problem: Given a DAG of `N` nodes and `M` edges where `N` could be up to `10^5`, there are `Q` queries, each query are two nodes `u` and `v`, you need to check if `v` is reachable from `u`.
The naive solution is to run a DFS or BFS for each query, which will take `O(N + M)` time per query, where `M` is the number of edges. This can lead to a total time complexity of `O(Q * (N + M))`, which is not efficient for large inputs. Another approach is to using dp with bitset where `dp[i]` store a bitset of size `N` indicating which nodes are reachable from node `i`. To build `dp`, you can use Kahn's algorithm.
```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAX_N = 5000; 
int main(){
  int n, m; cin>>n>>m;
  vector<vector<int>> g(n);
  vector<int> in_degree(n);
  for(int i = 0; i < m; i++){
    int u, v; cin>>u>>v;
    g[u].push_back(v);
    in_degree[v]++;
  }
  queue<int> q;
  vector<bitset<MAX_N>> dp(n);
  for(int i = 0; i < n; i++){
    if(in_degree[i] == 0)
      q.push(i);
    dp[i][i] = 1;  // each node can reach itself
  }
  while(!empty(q)){
    int u = q.front(); q.pop();
    for(int v : g[u]){
      dp[v] |= dp[u];  // Update dp[v] with reachability from u
      if(--in_degree[v] == 0)
        q.push(v);
    }
  }
  int Q; cin>>Q;
  while(Q--){
    int u, v; cin>>u>>v;
    cout<<(dp[u][v] ? "YES\n" : "NO\n");
  }
  return 0;
}
```

The operations on bitsets are very efficient. The time complexity of the `or` operation on two bitsets of size `N` is `O(N / 32)` or `O(N / 64)` depending on the architecture, which is much faster than iterating through all nodes. The overall time complexity for building the `dp` table using this method is `O(N^2 / 64 + M)` and then you can answer each query in `O(1)` time. This might work with generous time limits, but this method raises another problem: the size of `dp` could be too large for large `N`. If `N = 100000`, you need `N^2` bits which could lead to memory issues.(1)

To solve this problem, we can use the block-of-64 trick. The idea is to divide the nodes into blocks of size 64 and use a bitset for each block. This way, we can reduce the memory usage significantly. Here is how you can implement this:

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
  int n, m; cin>>n>>m;
  vector<vector<int>> g(n);
  vector<int> in_degree(n);
  for(int i = 0; i < m; i++){
    int u, v; cin>>u>>v;
    g[u].push_back(v);
    in_degree[v]++;
  }
  queue<int> q;
  vector<int> order;
  for(int i = 0; i < n; i++)
    if(in_degree[i] == 0){
      q.push(i);
      order.push_back(i);
    }
  while(!empty(q)){
    int u = q.front(); q.pop();
    for(auto v : g[u])
      if(--in_degree[v] == 0){
        q.push(v);
        order.push_back(v);
      }
  }
  int Q; cin>>Q;
  vector<pair<int, int>> queries;
  for(int i = 0; i < Q; i++){
    int u, v; cin>>u>>v;
    queries.push_back({u, v}); // can you reach v from u?
  }
  vector<int> ans(Q, 0);
  const int block_size = 64;
  for(int block_start = 0; block_start < n; block_start += block_size){
    int block_end = min(n, block_start + block_size);
    vector<bitset<block_size>> dp(n);
    for(auto u : order){
      if(block_start <= u && u < block_end)
        dp[u][u - block_start] = 1;
      for(auto v : g[u])
        dp[v] |= dp[u];
    }
    for(int q = 0; q < Q; q++){
      auto [u, v] = queries[q];
      if(block_start <= v && v < block_end && dp[u][v - block_start])
        ans[q] = 1;
    }
  }
  for(int q = 0; q < Q; q++)
    cout<<(ans[q] ? "YES" : "NO")<<"\n";
  return 0;
}
```
Note:

(1) We are using 10^5 bitsets of size 10^5 which takes 10^10 bits = 1.25GB of memory.