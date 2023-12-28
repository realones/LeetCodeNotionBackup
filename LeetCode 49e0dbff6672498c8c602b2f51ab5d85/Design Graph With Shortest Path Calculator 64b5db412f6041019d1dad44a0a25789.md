# Design Graph With Shortest Path Calculator

#: 2642
Difficult: Hard
Tags: Design, Dijkstra, Graph, PQ, shortest_path
link: https://leetcode.com/problems/design-graph-with-shortest-path-calculator/
Priority: Medium
Status: checkBetterSolution
Created time: November 11, 2023 11:49 PM
Last edited time: November 11, 2023 11:51 PM
source: leetcode_daily

There is a **directed weighted** graph that consists of `n` nodes numbered from `0` to `n - 1`. The edges of the graph are initially represented by the given array `edges` where `edges[i] = [fromi, toi, edgeCosti]` meaning that there is an edge from `fromi` to `toi` with the cost `edgeCosti`.

Implement the `Graph` class:

- `Graph(int n, int[][] edges)` initializes the object with `n` nodes and the given edges.
- `addEdge(int[] edge)` adds an edge to the list of edges where `edge = [from, to, edgeCost]`. It is guaranteed that there is no edge between the two nodes before adding this one.
- `int shortestPath(int node1, int node2)` returns the **minimum** cost of a path from `node1` to `node2`. If no path exists, return `1`. The cost of a path is the sum of the costs of the edges in the path.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/01/11/graph3drawio-2.png](https://assets.leetcode.com/uploads/2023/01/11/graph3drawio-2.png)

```
Input
["Graph", "shortestPath", "shortestPath", "addEdge", "shortestPath"]
[[4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]], [3, 2], [0, 3], [[1, 3, 4]], [0, 3]]
Output
[null, 6, -1, null, 6]

Explanation
Graph g = new Graph(4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]);
g.shortestPath(3, 2); // return 6. The shortest path from 3 to 2 in the first diagram above is 3 -> 0 -> 1 -> 2 with a total cost of 3 + 2 + 1 = 6.
g.shortestPath(0, 3); // return -1. There is no path from 0 to 3.
g.addEdge([1, 3, 4]); // We add an edge from node 1 to node 3, and we get the second diagram above.
g.shortestPath(0, 3); // return 6. The shortest path from 0 to 3 now is 0 -> 1 -> 3 with a total cost of 2 + 4 = 6.

```

**Constraints:**

- `1 <= n <= 100`
- `0 <= edges.length <= n * (n - 1)`
- `edges[i].length == edge.length == 3`
- `0 <= fromi, toi, from, to, node1, node2 <= n - 1`
- `1 <= edgeCosti, edgeCost <= 106`
- There are no repeated edges and no self-loops in the graph at any point.
- At most `100` calls will be made for `addEdge`.
- At most `100` calls will be made for `shortestPath`.

comment: beat 6%, so check better solution

```cpp
class Graph {
public:
    unordered_map<int, unordered_map<int,int>> g;
    int n=0;
    Graph(int n, vector<vector<int>>& edges) {
        for(auto &e: edges){
            int u=e[0], v=e[1], w=e[2];
            g[u][v]=w;
        }
        this->n=n;
    }
    
    void addEdge(vector<int> e) {
        int u=e[0], v=e[1], w=e[2];
        g[u][v]=w;
    }
    
    //dijkstra - O(nlogn) * 100
    using ii=pair<int,int>;
    int shortestPath(int node1, int node2) {
        priority_queue<ii,vector<ii>,greater<>> pq;//[(d,u)]
        vector<int> dist(n,INT_MAX);

        pq.push({0,node1});
        while(!pq.empty()){
            auto [d,u] = pq.top(); pq.pop();
            if(dist[u]!=INT_MAX) continue;
            dist[u]=d;
            
            for(auto [v,w]: g[u]){
                if(dist[v]!=INT_MAX) continue;
                pq.push({d+w,v});
            }
        }
        return dist[node2]==INT_MAX?-1:dist[node2];
    }
};

/**
 * Your Graph object will be instantiated and called as such:
 * Graph* obj = new Graph(n, edges);
 * obj->addEdge(edge);
 * int param_2 = obj->shortestPath(node1,node2);
 */
```