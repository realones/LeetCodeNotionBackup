# Reachable Nodes In Subdivided Graph

#: 882
Difficult: Hard
Tags: Dijkstra, Graph, PQ
link: https://leetcode.com/problems/reachable-nodes-in-subdivided-graph/
Priority: High
Status: HardToImplement, No Idea At First
Created time: June 28, 2023 12:24 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given an undirected graph (the **"original graph"**) with `n` nodes labeled from `0` to `n - 1`. You decide to **subdivide** each edge in the graph into a chain of nodes, with the number of new nodes varying between each edge.

The graph is given as a 2D array of `edges` where `edges[i] = [ui, vi, cnti]` indicates that there is an edge between nodes `ui` and `vi` in the original graph, and `cnti` is the total number of new nodes that you will **subdivide** the edge into. Note that `cnti == 0` means you will not subdivide the edge.

To **subdivide** the edge `[ui, vi]`, replace it with `(cnti + 1)` new edges and `cnti` new nodes. The new nodes are `x1`, `x2`, ..., `xcnti`, and the new edges are `[ui, x1]`, `[x1, x2]`, `[x2, x3]`, ..., `[xcnti-1, xcnti]`, `[xcnti, vi]`.

In this **new graph**, you want to know how many nodes are **reachable** from the node `0`, where a node is **reachable** if the distance is `maxMoves` or less.

Given the original graph and `maxMoves`, return *the number of nodes that are **reachable** from node* `0` *in the new graph*.

**Example 1:**

![https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/01/origfinal.png](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/01/origfinal.png)

```
Input: edges = [[0,1,10],[0,2,1],[1,2,2]], maxMoves = 6, n = 3
Output: 13
Explanation: The edge subdivisions are shown in the image above.
The nodes that are reachable are highlighted in yellow.

```

**Example 2:**

```
Input: edges = [[0,1,4],[1,2,6],[0,2,8],[1,3,1]], maxMoves = 10, n = 4
Output: 23

```

**Example 3:**

```
Input: edges = [[1,2,4],[1,4,5],[1,3,1],[2,3,4],[3,4,5]], maxMoves = 17, n = 5
Output: 1
Explanation: Node 0 is disconnected from the rest of the graph, so only node 0 is reachable.

```

**Constraints:**

- `0 <= edges.length <= min(n * (n - 1) / 2, 104)`
- `edges[i].length == 3`
- `0 <= ui < vi < n`
- There are **no multiple edges** in the graph.
- `0 <= cnti <= 104`
- `0 <= maxMoves <= 109`
- `1 <= n <= 3000`

comment: 

dijkstra is easy to figure out, but you need to really understand how dijkstra works:

in the first loop

```cpp
while(!pq.empty()){
		auto [d,u] = pq.top(); pq.pop();
		if(dist[u]!=INT_MAX) continue;
```

it will touch every node with redundant, with `if(dist[u]!=INT_MAX) continue;` to cut redundant.

then for each node, in the `for(auto [v,w]: g[u])` loop it will touch every edge from that node, where you can check if you can push node `v` into pq, here we check if `v` is valid by checking the `maxMoves`, and calculate how many moves left from `u→v`.

Solution:

be careful that the actual dist from u to v is w+1

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int reachableNodes(vector<vector<int>>& edges, int maxMoves, int n) {
        vector<unordered_map<int,int>> g(n);
        for(auto& e: edges){
            int u=e[0], v=e[1], w=e[2];
            g[u][v]=g[v][u]=w;
        }

        unordered_map<int,unordered_map<int,int>> used;
        priority_queue<ii,vector<ii>,greater<>> pq;
        vector<int> dist(n,INT_MAX);

        pq.push({0,0});//dist,node
        int res=0;
        while(!pq.empty()){
            auto [d,u] = pq.top(); pq.pop();
            if(dist[u]!=INT_MAX) continue;
            dist[u]=d;
            res++;

            for(auto [v,w]: g[u]){
                used[u][v] = min(w, maxMoves-d);
                if(dist[v]!=INT_MAX) continue;
                if(maxMoves-d < w+1) continue;//not enough moves, w+1 is the actual dist of u->v
                pq.push({d+w+1,v});//w+1 since edge=new_node+1
            }
        }

        for(auto& e: edges){
            int u=e[0], v=e[1], w=e[2];
            res+=min(w, used[u][v]+used[v][u]);
        }
        return res;
    }
};

/*
dijkstra, find shortest path from 0 to all nodes

while we do dijkstra, for each edge that try pushing v into pq
    maintain how much moves we can use on the edge
        moves of edge(u->v) = min(w[u->v], maxMoves - dist[u])

then sum up the moves can used in all edges
*/
```