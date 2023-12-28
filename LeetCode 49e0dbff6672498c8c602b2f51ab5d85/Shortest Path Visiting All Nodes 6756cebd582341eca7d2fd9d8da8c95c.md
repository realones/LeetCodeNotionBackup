# Shortest Path Visiting All Nodes

#: 847
Difficult: Hard
Tags: BFS, Bit Manipulation, BitMask, DP, Graph, visiting_all_nodes_with_dup
link: https://leetcode.com/problems/shortest-path-visiting-all-nodes/description/
Priority: High
Status: More Solution, No Idea At First, toRevisit
Created time: August 31, 2023 11:28 AM
Last edited time: October 17, 2023 6:24 PM
source: HuaHua
related: Shortest Path to Get All Keys (Shortest%20Path%20to%20Get%20All%20Keys%203b99ac140bc54609a38f4a0b183344fa.md)

You have an undirected, connected graph of `n` nodes labeled from `0` to `n - 1`. You are given an array `graph` where `graph[i]` is a list of all the nodes connected with node `i` by an edge.

Return *the length of the shortest path that visits every node*. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/05/12/shortest1-graph.jpg](https://assets.leetcode.com/uploads/2021/05/12/shortest1-graph.jpg)

```
Input: graph = [[1,2,3],[0],[0],[0]]
Output: 4
Explanation: One possible path is [1,0,2,0,3]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/05/12/shortest2-graph.jpg](https://assets.leetcode.com/uploads/2021/05/12/shortest2-graph.jpg)

```
Input: graph = [[1],[0,2,4],[1,3,4],[2],[1,2]]
Output: 4
Explanation: One possible path is [0,1,4,2,3]

```

**Constraints:**

- `n == graph.length`
- `1 <= n <= 12`
- `0 <= graph[i].length < n`
- `graph[i]` does not contain `i`.
- If `graph[a]` contains `b`, then `graph[b]` contains `a`.
- The input graph is always connected.

comment: not quite understand why the BFS works.

see similar questions, there are permutation + BFS solution

Solution BFS:

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int shortestPathLength(vector<vector<int>>& graph) {
        //bfs
        int n=graph.size();
        vector<vector<int>> visited(n,vector<int>(1<<n,0));
        queue<ii> q;

        for(int i=0; i<n; i++){
            visited[i][1<<i]=true;
            q.push({i,1<<i});
        }
        
        int step=0;
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                auto [node, state]=q.front(); q.pop();
                if(state==(1<<n)-1) return step;
                //process
                for(auto nei: graph[node]){
                    if(visited[nei][state|(1<<nei)]) continue;
                    visited[nei][state|(1<<nei)]=true;
                    q.push({nei,state|(1<<nei)});
                }    
            }
            step++;
        }
        return step;
    }
};

/*
undirected unweighted graph
shortest path to visited all nodes
can revisit edges and nodes
================================
wisdompeak
search -> how to dedup - mark ? as visited
since we can go through same node and edge, can't just store node /edge
make all visited nodes as a state? - still dup

state: (curNode + visited_nodes)
visited[12][1<<12]

BFS, init with {i,1<<i} i in [0,n-1]

*/
```