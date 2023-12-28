# Critical Connections in a Network

#: 1192
Difficult: Hard
Tags: Biconnected Component, DFS, Graph, Tarjan, Tree, circle
link: https://leetcode.com/problems/critical-connections-in-a-network/
Priority: High
Status: Dig More, HardToImplement, No Idea At First, toRevisit
Created time: September 2, 2023 4:45 PM
Last edited time: October 17, 2023 6:24 PM
source: HuaHua

There are `n` servers numbered from `0` to `n - 1` connected by undirected server-to-server `connections` forming a network where `connections[i] = [ai, bi]` represents a connection between servers `ai` and `bi`. Any server can reach other servers directly or indirectly through the network.

A *critical connection* is a connection that, if removed, will make some servers unable to reach some other server.

Return all critical connections in the network in any order.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png](https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png)

```
Input: n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
Output: [[1,3]]
Explanation: [[3,1]] is also accepted.

```

**Example 2:**

```
Input: n = 2, connections = [[0,1]]
Output: [[0,1]]

```

**Constraints:**

- `2 <= n <= 105`
- `n - 1 <= connections.length <= 105`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- There are no repeated connections.

comment:

new algorithm: Tarjan

dfs思路好想，但是实现起来很难，感觉cur用到了future的信息，而future又depend on cur calculation。

Solution: not quite sure how it works…

Time complexity: O(v+e)

Space complexity: O(v+e)

以任意一个未访问过的节点作为根节点，用DFS的顺序来进行搜索，即永远深度优先，然后回溯再搜索其他分支。如果碰到访问过的节点，就停止，保证不行成环。

```cpp
class Solution {
public:
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        //build graph
        vector<unordered_set<int>> g(n);
        for(auto& e: connections){
            g[e[0]].insert(e[1]);
            g[e[1]].insert(e[0]);
        }
        //define tarjan
        vector<vector<int>> res;
        vector<int> minDepths(n,INT_MAX);//min depth for each node
        //u->v: depth++, u<-v: minDepths[cur] min= minDepths[nxt], so
        //  -for nodes in same circle, mimDepth are same
        //  -for bridges, from u->v, the minDepth will +1
        function<int(int,int,int)> tarjan = [&](int parent, int u, int depth)->int {//return midDepth
            if(minDepths[u]!=INT_MAX) return minDepths[u];//visited+dp
            minDepths[u]=depth;//init
            for(auto v: g[u]){
                if(v==parent) continue;
                int vDepth=tarjan(u, v, depth+1);
                if(depth<minDepths[v]) res.push_back({u,v});
                minDepths[u]=min(minDepths[u], vDepth);
            }
            return minDepths[u];
        };
        //start dfs call and return
        tarjan(-1,0,0);//start from arbitrary node 0, init depth is 0, init dummy parent
        return res;
    }
};
```