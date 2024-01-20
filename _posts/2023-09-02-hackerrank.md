---
layout: post
title: "[Tech] Hackerrank problems writing-ups"
---

I just realized Hackerrank have a list of really problems that suit my taste. I prefer problems that are short, algorithmic and interview-type. Following are my writing-up of non-trivial problem solving on this platform!. 
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
