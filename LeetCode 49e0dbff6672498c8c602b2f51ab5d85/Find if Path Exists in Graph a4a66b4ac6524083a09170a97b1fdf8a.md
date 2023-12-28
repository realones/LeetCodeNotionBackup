# Find if Path Exists in Graph

#: 1971
Difficult: Medium
Tags: BFS, DFS, Graph, Union Find
link: https://leetcode.com/problems/find-if-path-exists-in-graph/description/
Priority: Low
Status: More Solution
Created time: December 13, 2023 1:45 AM
Last edited time: December 13, 2023 1:45 AM
source: microsoftFreq

There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n - 1` (**inclusive**). The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [ui, vi]` denotes a bi-directional edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by **at most one** edge, and no vertex has an edge to itself.

You want to determine if there is a **valid path** that exists from vertex `source` to vertex `destination`.

Given `edges` and the integers `n`, `source`, and `destination`, return `true` *if there is a **valid path** from* `source` *to* `destination`*, or* `false` *otherwise.*

**Example 1:**

![https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

```
Input: n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
Output: true
Explanation: There are two paths from vertex 0 to vertex 2:
- 0 → 1 → 2
- 0 → 2

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

```
Input: n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
Output: false
Explanation: There is no path from vertex 0 to vertex 5.

```

**Constraints:**

- `1 <= n <= 2 * 105`
- `0 <= edges.length <= 2 * 105`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- There are no duplicate edges.
- There are no self edges.

```cpp
class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int src, int des) {
        if(src==des) return true;
        //build graph
        unordered_map<int,unordered_set<int>> g;
        for(auto& e: edges){
            int u=e[0], v=e[1];
            g[u].insert(v);
            g[v].insert(u);
        }
        //bfs or dfs or unionFind
        queue<int> q;
        unordered_set<int> visited;
        visited.insert(src);
        q.push(src);

        while(!q.empty()){
            int u = q.front(); q.pop();
            if(u==des) return true;
            //process
            for(auto v: g[u]){
                if(visited.count(v)) continue;
                visited.insert(v);
                q.push(v);
            }
        }
        return false;
    }
};
```