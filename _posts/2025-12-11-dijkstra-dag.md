---
layout: post
title: "DAG on Dijkstra"
---

Dijkstra is a very well-know algorithm to find the shortest path from a single source node to all other nodes in a weighted graph with non-negative weights. One interesting property of Dijkstra's algorithm is that the shortest paths it finds form a Directed Acyclic Graph (DAG).

Let's say we are interested in finding the number of different shortest paths from the source node `s` to a target node `t`. Two paths are considered different if the sequence of nodes in the paths are different.

Using Dijkstra on a graph `G = (V, E)` with source node `s`. We have a distance array `dist[]` where `dist[u]` is the shortest distance from `s` to `u`. We now created a new **directed** graph `G' = (V, E')` where `E'` is defined as follows:
```E' = {(u -> v) | (u, v) ∈ E and dist[v] = dist[u] + weight(u, v)}```. Constructing a DAG usually a prior step to do dynamic programming on graphs.

To find the number of different shortest paths from `s` to all other nodes, it is sufficient to do a topological sort on `G'` and then do dynamic programming on the DAG. Here is the illustration:
```cpp
int main(){
    int n, m, s;
    vector<vector<pair<int, int>>> g;
    vector<int> dist(n, INT_MAX);
    // Reading the input, run Dijkstra on g from s to get dist[]
    vector<int> cnt(n);
    cnt[s] = 1; // number of paths to source is 1
    for(int i = 0; i < n; i++){
        for(auto [v, w] : g[i]){ 
            if(dist[v] == dist[i] + w){ // edge (i, v) is in E'
                cnt[v] += cnt[i]; // number of paths to v += number of paths to i
            }
        }
    }
}
```

In fact, we can combine the Dijkstra and DP steps into one. That means we can calculate `cnt` as we do Dijkstra. During the Dijkstra's algorithm, when we relax an edge `(u, v)`, if we find a shorter path to `v`, we update `dist[v]` and set `cnt[v] = cnt[u]`. If we find another shortest path to `v` with the same distance, we add `cnt[u]` to `cnt[v]`. Here is the illustration:

```cpp
int main(){
    int n, m, s; cin>>n>>m>>s;
    vector<vector<pair<int, int>>> g(n);
    for(int i = 0; i < m; i++){
        int u, v, w; cin>>u>>v>>w;
        g[u].push_back({v, w});
        g[v].push_back({u, w}); // for undirected graph
    }
    vector<int> dist(n, INT_MAX);
    vector<int> cnt(n, 0);
    dist[s] = 0;
    cnt[s] = 1;
    set<pair<int, int>> pq; // {distance, node}
    pq.push({0, s});
    while(!pq.empty()){
        auto [d, u] = *begin(pq);
        pq.erase(begin(pq));
        if(d > dist[u]) continue;
        for(auto [v, w] : g[u]){
            if(dist[v] > dist[u] + w){
                dist[v] = dist[u] + w;
                cnt[v] = cnt[u];
                pq.push({dist[v], v});
            } else if(dist[v] == dist[u] + w){
                cnt[v] += cnt[u];
            }
        }
    }
}
```
