---
layout: post
title: "[Tech] Problem solving updates"
---

Here are some miscellaneous updates on my Algorithmic problem solving learnings.

## [Find Longest Special Substring That Occurs Thrice II](https://leetcode.com/problems/find-longest-special-substring-that-occurs-thrice-ii/)
Here is the problem that I get to optimize multiple times.

A string is called special if it is made up of a single character. Given a string `S`, return the length of the longest special substring of `S` such that that substring occurs at least three times in `S`.

General idea: if `k` satisfies the requirement, certainly is `k + 1` satisfy the requirement so we binary search on answer. Now for a given value `k` how to check if it is special has at least 3 substring in `S` ?

### Brute force
Initialize a `map<string, int>` and create a helper function `isSpecial(l, r)` using set to check substring `S[l..r]` is special. For every substring `substr` of size `k`, update `cnt[substr] += 1` if it `isSpecial`, and return as soon as `cnt[substr] == 3`. This gives memory limit exceeded(MLE).

### Improving isSpecial
From `l` to `r` how to check if it is only 1 character. Putting in my `PrefixSums` template which helps to minor reduce `isSpecial` to `O(26)`. It stills gives MLE because `substr` is too much and carrying `map<string, int>` is too heavy.

### Using hash
By using some precomputed hash, we can represent a substring `S[l..r]` by a number in `O(1)` time, our counter is now `map<long long, int>`. It now gives Time limit exceeded(TLE)

### Replacing prefix sum using dp
Now called `dp[i]` as length of largest substring using character `S[i]` using character before `i`. Precalculated `dp` in `O(N)`. We can now check if `S[l..r]` is special in O(1) by check `dp[r] - dp[l] + 1 == r - l + 1`. This gives `Accepted` verdict.

## Modeling grid as graph and run Dijsktra
### [Find a safe walk through a grid](https://leetcode.com/problems/find-a-safe-walk-through-a-grid/)
Given a binary grid `grid` size R x C. Starting from cell `(0, 0)` with initial health value `health`, you can going to 4 adjacent cell. If you go in a cell of value 1, your health get minused 1. Return `true` if you can get to `(R - 1, C - 1)` with positive health. 
### Solution
Modeling grid as graph with vertices are those *R * C* cells. Edges are 4 ways to adjacent cells with according weights if the cell is 1.

Running a dijkstra from `(0, 0)` to find the minimum health to get to `(R - 1, C - 1)`. Return true if the health needed is smaller than health. Make sure to decrement your initial health by 1 if `(0, 0)` is 1.

The time complexity is `O(ELogV)` because it is Dijkstra.

The solution with better time complexity is to invent a 2D array `max_health` where `max_health[r][c]` is the maximum health when you reach cell `(r, c)`, when enter `(r, c)` you will only proceed if your health is bigger than `max_health[r][c]`. The time complexy is now just `O(R * C)` 

### [Minimum cost to make at least one valid path in a grid](https://leetcode.com/problems/minimum-cost-to-make-at-least-one-valid-path-in-a-grid/)

You have a grid of size `R * C`, at a cell there is arrow pointing in one of the four ways indicating the way you can going towards. You can modify a sign at a cell with cost = 1. Return the minimum cost that you need to make to get from `(0, 0)` to `(R - 1, C - 1)`.
### Solution

A hard labeled problem. But basically the same idea as above. Making a graph having `R * C` vertices and `R * C * 4` edges with cost of `0` where it the initial way the sign the current is pointing to and cost of `1` if it is not. Running dijkstra from `(0, 0)` and return the cost at `(R - 1, C - 1)`.

### Finding cycle in a N vertice N edge graph
Given a graph having `N` vertices and `N` edges with no multiple edges. This particular type of graph has exactly one cycle. Finding this cycle is important for later steps of the problem.
```cpp
    vector<int> stack;
    vector<int> cycle;
    vector<bool> visited(N, false);
 
    function<bool(int, int)> dfs = [&](int node, int parent) -> bool {
        if (visited[node]) {
            cycle = {node};
 
            while (stack.back() != node) {
                cycle.push_back(stack.back());
                stack.pop_back();
            }
 
            return true;
        }
 
        stack.push_back(node);
        visited[node] = true;
 
        for (int neigh : adj[node])
            if (neigh != parent && dfs(neigh, node))
                return true;
 
        stack.pop_back();
        return false;
    };
    dfs(0, -1);
```

### [Sudoku solver](https://leetcode.com/problems/sudoku-solver)
Solve a sudoku of a `board` size of 9x9. I spend a noon writing code solving this.

Building a `vector<pair<int, int>, vector<int>>` where first values are the position of the empty cells, and second values are all possible numbers you can put at the according position.

Running a DFS from index 1. Try to put every value to the next positition. And when the index reach `N` you check if the resulting `board` is a valid sudoku, you return that board. This gives TLE.

A better solution is to check when every number you putting in, if that number is same with any number in the row, column or 3x3 grid, you can early stop here. This gives Accepted with 5% 