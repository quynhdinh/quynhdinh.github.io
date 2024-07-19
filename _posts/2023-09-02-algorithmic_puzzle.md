---
layout: post
title: "[Tech] Algorithmic puzzle writing-ups"
---

This is a place where I write up the solutions for some nice problems I stumbled upon

My problem selection taste are short, algorithmic and interview-type.
### [Cut the tree](https://www.hackerrank.com/challenges/cut-the-tree/problem)
Given a tree. Remove any one edge. Find the minimum absolute sum of two subtree value nodes.
```cpp
int cutTheTree(vector<int> data, vector<vector<int>> edges) {
    unordered_map<int, vector<int>> g;
    for(auto e: edges){
        g[e[0] - 1].push_back(e[1] - 1);
        g[e[1] - 1].push_back(e[0] - 1);
    }
    int n = data.size();
    queue<pair<int, int>> q({{0, -1}});
    stack<pair<int, int>> child2par;
    while(!q.empty()){
        auto [cur, par] = q.front();
        q.pop();
        for(int nxt: g[cur]){
            if(nxt != par){
                child2par.push({nxt, cur});
                q.push({nxt, cur});
            }
        }
    }
    while(!child2par.empty()){
        auto [child, par] = child2par.top();
        child2par.pop();
        data[par] += data[child];
    }
    int res = INT_MAX;
    for(int i = 1; i < n; i++){
        res = min(res, abs((data[0] - data[i]) - data[i]));
    }
    return res;
}
```

### [Shortest path 3](https://atcoder.jp/contests/abc362/tasks/abc362_d)
Given an undirected graph. Each vertice and an edge has a cost associated with it. The weight of a path of two vertices is the sum of the cost of edge and sum of all vertices appear on the path.

Starting from vertice 1. Find the shortest path to all other vertices.

Solution [here](https://atcoder.jp/contests/abc362/editorial/10422)

