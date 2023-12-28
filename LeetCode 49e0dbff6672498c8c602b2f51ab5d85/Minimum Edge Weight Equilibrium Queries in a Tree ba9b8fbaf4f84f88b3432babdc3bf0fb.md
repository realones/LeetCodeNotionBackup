# Minimum Edge Weight Equilibrium Queries in a Tree

#: 2846
Difficult: Hard
Tags: BinaryLifting, Graph, Tarjan, Tree
link: https://leetcode.com/problems/minimum-edge-weight-equilibrium-queries-in-a-tree/description/
Priority: High
Status: Dig More, More Solution, checkBetterSolution, toRevisit
Created time: September 4, 2023 4:14 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests
related: Lowest Common Ancestor of a Binary Tree (Lowest%20Common%20Ancestor%20of%20a%20Binary%20Tree%20358623c63e324e6e9d5de06b9aa3ec65.md), Count Valid Paths in a Tree (Count%20Valid%20Paths%20in%20a%20Tree%2033d846368c45442893e6a96adb7c17ac.md)

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ui, vi, wi]` indicates that there is an edge between nodes `ui` and `vi` with weight `wi` in the tree.

You are also given a 2D integer array `queries` of length `m`, where `queries[i] = [ai, bi]`. For each query, find the **minimum number of operations** required to make the weight of every edge on the path from `ai` to `bi` equal. In one operation, you can choose any edge of the tree and change its weight to any value.

**Note** that:

- Queries are **independent** of each other, meaning that the tree returns to its **initial state** on each new query.
- The path from `ai` to `bi` is a sequence of **distinct** nodes starting with node `ai` and ending with node `bi` such that every two adjacent nodes in the sequence share an edge in the tree.

Return *an array* `answer` *of length* `m` *where* `answer[i]` *is the answer to the* `ith` *query.*

**Example 1:**

![https://assets.leetcode.com/uploads/2023/08/11/graph-6-1.png](https://assets.leetcode.com/uploads/2023/08/11/graph-6-1.png)

```
Input: n = 7, edges = [[0,1,1],[1,2,1],[2,3,1],[3,4,2],[4,5,2],[5,6,2]], queries = [[0,3],[3,6],[2,6],[0,6]]
Output: [0,0,1,3]
Explanation: In the first query, all the edges in the path from 0 to 3 have a weight of 1. Hence, the answer is 0.
In the second query, all the edges in the path from 3 to 6 have a weight of 2. Hence, the answer is 0.
In the third query, we change the weight of edge [2,3] to 2. After this operation, all the edges in the path from 2 to 6 have a weight of 2. Hence, the answer is 1.
In the fourth query, we change the weights of edges [0,1], [1,2] and [2,3] to 2. After these operations, all the edges in the path from 0 to 6 have a weight of 2. Hence, the answer is 3.
For each queries[i], it can be shown that answer[i] is the minimum number of operations needed to equalize all the edge weights in the path from ai to bi.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/08/11/graph-9-1.png](https://assets.leetcode.com/uploads/2023/08/11/graph-9-1.png)

```
Input: n = 8, edges = [[1,2,6],[1,3,4],[2,4,6],[2,5,3],[3,6,6],[3,0,8],[7,0,2]], queries = [[4,6],[0,4],[6,5],[7,4]]
Output: [1,2,2,3]
Explanation: In the first query, we change the weight of edge [1,3] to 6. After this operation, all the edges in the path from 4 to 6 have a weight of 6. Hence, the answer is 1.
In the second query, we change the weight of edges [0,3] and [3,1] to 6. After these operations, all the edges in the path from 0 to 4 have a weight of 6. Hence, the answer is 2.
In the third query, we change the weight of edges [1,3] and [5,2] to 6. After these operations, all the edges in the path from 6 to 5 have a weight of 6. Hence, the answer is 2.
In the fourth query, we change the weights of edges [0,7], [0,3] and [1,3] to 6. After these operations, all the edges in the path from 7 to 4 have a weight of 6. Hence, the answer is 3.
For each queries[i], it can be shown that answer[i] is the minimum number of operations needed to equalize all the edge weights in the path from ai to bi.

```

**Constraints:**

- `1 <= n <= 104`
- `edges.length == n - 1`
- `edges[i].length == 3`
- `0 <= ui, vi < n`
- `1 <= wi <= 26`
- The input is generated such that `edges` represents a valid tree.
- `1 <= queries.length == m <= 2 * 104`
- `queries[i].length == 2`
- `0 <= ai, bi < n`

comment: from “Kth Ancestor of a tree node”, through “Lowest Common Ancestor of a Binary Tree” to this questions solution, a long way to figure out the solution. also the performance only beats 10%, need to check better solutions.

also, why tarjan tag?

there should be more solutions as well.

Solution: binary lifting

```cpp
class Solution {
public:
    //this is code from "LCA"
    int n;//num of node
    int M;//max bit that 2^M 
    vector<vector<int>> dp;//2^i step up, j idx -- target idx
    vector<int> depth;
    vector<int> parent;
    vector<unordered_map<int,int>> g;//u,v,w
    vector<unordered_map<int,int>> prefix_w_freq;//u,w,cnt

    void initTreeAncestor() {
        M=log2(n);
        dp.resize(M+1, vector<int>(n,0));
        //init
        for(int j=0;j<n;j++){
            dp[0][j]=parent[j];
        }

        for(int i=1;i<M+1;i++){
            for(int j=0;j<n;j++){
                int j2=dp[i-1][j];
                dp[i][j] = j2==-1?-1:dp[i-1][j2];
            }
        }
    }
    
    int getKthAncestor(int node, int k) {
        for(int i=0; i<M+1; i++){
            if(k & (1<<i)){
                if(node==-1) return -1;
                node=dp[i][node];
            }
        }
        return node;
    }
    
    int getLca(int node1, int node2) {
        //ensure both nodes are on the same level
        if(depth[node1] > depth[node2]) swap(node1,node2);
        node2=getKthAncestor(node2, depth[node2]-depth[node1]);
        if(node1==node2) return node1;
        //jump from large to small steps to find the lca
        for(int i=M; i>=0; i--){//2^i steps up
            int node12=dp[i][node1];
            int node22=dp[i][node2];
            if(node12==node22) continue;
            else{
                node1=node12;
                node2=node22;
            }
        }
        return parent[node1];
    }

    void dfs(int p, int u, int d){//parent, cur, depth
        parent[u]=p;
        depth[u]=d;
        for(auto [v,w]: g[u]){
            if(v==p) continue;//dont go back
            prefix_w_freq[v]=prefix_w_freq[u];
            prefix_w_freq[v][w]++;
            dfs(u, v, d+1);
        }
    }

    vector<int> minOperationsQueries(int n, vector<vector<int>>& edges, vector<vector<int>>& queries) {
        this->n=n;
        g.resize(n);
        prefix_w_freq.resize(n);
        parent.resize(n);
        depth.resize(n);
        //build graph
        for(auto& e: edges){
            int u=e[0], v=e[1], w=e[2];
            g[u][v]=w;
            g[v][u]=w;
        }
        //preprocess
        dfs(-1,0,0);//p u w, use node 0 as root, with -1 as it's parent
        //prepare dp
        initTreeAncestor();

        //f: res from u->v
        function<int(int,int)> f = [&](int u, int v) -> int{
            if(u==v) return 0;
            int lca=getLca(u, v);
            //dist from u->v
            int dist=depth[u]+depth[v] - 2*depth[lca];
            //max freq of weight from u->v
            int max_w_freq=0;
            for(int w=1; w<=26; w++){
                int freq1=prefix_w_freq[u][w] - prefix_w_freq[lca][w];
                int freq2=prefix_w_freq[v][w] - prefix_w_freq[lca][w];
                max_w_freq=max(max_w_freq, freq1+freq2);
            }
            return dist-max_w_freq;
        };
        //calculate the query
        vector<int> res;
        for(auto& q: queries){
            int u=q[0], v=q[1];
            res.push_back(f(u,v));
        }
        return res;
    }
};

/*
V: 1~1e4
E: V-1, valid tree
w: 1~26
queryCnt: 1~2e4

================================
binary lifting - from RealFan
https://leetcode.com/problems/minimum-edge-weight-equilibrium-queries-in-a-tree/solutions/3996073/c-java-python-prefix-sum-and-lca-on-the-tree-explained-in-detail/

intui:
    path between two nodes - lca
        path = path(lca,node1) + path(lca,node2)
    min changes of weight - maintain the max freq of weight x, and change others
        keep a mp[w,freq], since w is 1~26, very small, maintain all w freq in the map
    
    preprocess dp for binary lifting: O(nlogn)
    find lca -> binary lifting: O(logn)

    mp[w,freq] from node to lca
*/
```