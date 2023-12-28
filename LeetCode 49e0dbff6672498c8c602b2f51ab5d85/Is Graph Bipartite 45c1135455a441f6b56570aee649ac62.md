# Is Graph Bipartite?

#: 785
Difficult: Medium
Tags: BFS, Bipartition_graphColoring, DFS, Graph, Union Find
link: https://leetcode.com/problems/is-graph-bipartite/description/
Priority: Medium
Status: More Solution
Created time: August 24, 2023 4:39 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Possible Bipartition (Possible%20Bipartition%2091625744774f4c66a944f881af8611a8.md), Flower Planting With No Adjacent (Flower%20Planting%20With%20No%20Adjacent%20fee61ff1b1164d9eb2b6922355e3e5d8.md)

There is an **undirected** graph with `n` nodes, where each node is numbered between `0` and `n - 1`. You are given a 2D array `graph`, where `graph[u]` is an array of nodes that node `u` is adjacent to. More formally, for each `v` in `graph[u]`, there is an undirected edge between node `u` and node `v`. The graph has the following properties:

- There are no self-edges (`graph[u]` does not contain `u`).
- There are no parallel edges (`graph[u]` does not contain duplicate values).
- If `v` is in `graph[u]`, then `u` is in `graph[v]` (the graph is undirected).
- The graph may not be connected, meaning there may be two nodes `u` and `v` such that there is no path between them.

A graph is **bipartite** if the nodes can be partitioned into two independent sets `A` and `B` such that **every** edge in the graph connects a node in set `A` and a node in set `B`.

Return `true` *if and only if it is **bipartite***.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/21/bi2.jpg](https://assets.leetcode.com/uploads/2020/10/21/bi2.jpg)

```
Input: graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
Output: false
Explanation: There is no way to partition the nodes into two independent sets such that every edge connects a node in one and a node in the other.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/10/21/bi1.jpg](https://assets.leetcode.com/uploads/2020/10/21/bi1.jpg)

```
Input: graph = [[1,3],[0,2],[1,3],[0,2]]
Output: true
Explanation: We can partition the nodes into two sets: {0, 2} and {1, 3}.
```

**Constraints:**

- `graph.length == n`
- `1 <= n <= 100`
- `0 <= graph[u].length < n`
- `0 <= graph[u][i] <= n - 1`
- `graph[u]` does not contain `u`.
- All the values of `graph[u]` are **unique**.
- If `graph[u]` contains `v`, then `graph[v]` contains `u`.

comment: 思路比较新颖

Solution BFS:

分成不同的connected component单独处理，每个bfs基础上做点小check来保证没有conflict

```cpp
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n=graph.size();
        unordered_map<int,int> mp;//visited: idx_1/-1 1meansA, -1meansB
        for(int i=0; i<n; i++){//since can be more than 1 connected component
            if(mp.count(i)) continue;
            if(!bfs(graph,mp,i)) return false;
        }
        return true;
    }

    bool bfs(vector<vector<int>>& graph, unordered_map<int,int>& mp, int start){
        int n=graph.size();
        
        queue<int> q;
        mp[start]=1;
        q.push(start);

        while(!q.empty()){
            int cur=q.front(); q.pop();
            //process
            for(auto nei: graph[cur]){
                if(mp.count(nei)) {
                    if(mp[cur]*mp[nei]==1) return false;//check if contradict
                    continue;
                }
                mp[nei]=mp[cur]*(-1);
                q.push(nei);
            }
        }
        return true;
    }
};

/*
n nodes: 0...n-1
g[u]: u->[v]

properties:
- no self edges
- no parallel edges
- u->v then v->u
- can be not connected

return if 
    the nodes can be partitioned into two independent sets A and B
    ->
    every edge connects a node in set A and a node in set B.

for each connected component
    from any point
    A -> all Bs -> all As -> can't have both A and B

maintain:
    st A
    st B

BFS/DFS

*/
```