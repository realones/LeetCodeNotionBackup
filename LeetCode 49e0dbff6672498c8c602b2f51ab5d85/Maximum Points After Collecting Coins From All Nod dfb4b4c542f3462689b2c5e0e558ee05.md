# Maximum Points After Collecting Coins From All Nodes

#: 2920
Difficult: Hard
Tags: DFS, DP, Graph, Math, Tree
link: https://leetcode.com/problems/maximum-points-after-collecting-coins-from-all-nodes/description/
Priority: High
Status: No Idea At First, toRevisit
Created time: October 29, 2023 12:35 AM
Last edited time: October 29, 2023 2:30 PM
source: LC_Contests

There exists an undirected tree rooted at node `0` with `n` nodes labeled from `0` to `n - 1`. You are given a 2D **integer** array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree. You are also given a **0-indexed** array `coins` of size `n` where `coins[i]` indicates the number of coins in the vertex `i`, and an integer `k`.

Starting from the root, you have to collect all the coins such that the coins at a node can only be collected if the coins of its ancestors have been already collected.

Coins at `nodei` can be collected in one of the following ways:

- Collect all the coins, but you will get `coins[i] - k` points. If `coins[i] - k` is negative then you will lose `abs(coins[i] - k)` points.
- Collect all the coins, but you will get `floor(coins[i] / 2)` points. If this way is used, then for all the `nodej` present in the subtree of `nodei`, `coins[j]` will get reduced to `floor(coins[j] / 2)`.

Return *the **maximum points** you can get after collecting the coins from **all** the tree nodes.*

**Example 1:**

![https://assets.leetcode.com/uploads/2023/09/18/ex1-copy.png](https://assets.leetcode.com/uploads/2023/09/18/ex1-copy.png)

```
Input: edges = [[0,1],[1,2],[2,3]], coins = [10,10,3,3], k = 5
Output: 11
Explanation:
Collect all the coins from node 0 using the first way. Total points = 10 - 5 = 5.
Collect all the coins from node 1 using the first way. Total points = 5 + (10 - 5) = 10.
Collect all the coins from node 2 using the second way so coins left at node 3 will be floor(3 / 2) = 1. Total points = 10 + floor(3 / 2) = 11.
Collect all the coins from node 3 using the second way. Total points = 11 + floor(1 / 2) = 11.
It can be shown that the maximum points we can get after collecting coins from all the nodes is 11.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/09/18/ex2.png](https://assets.leetcode.com/uploads/2023/09/18/ex2.png)

```
Input: edges = [[0,1],[0,2]], coins = [8,4,4], k = 0
Output: 16
Explanation:
Coins will be collected from all the nodes using the first way. Therefore, total points = (8 - 0) + (4 - 0) + (4 - 0) = 16.

```

**Constraints:**

- `n == coins.length`
- `2 <= n <= 105`
- `0 <= coins[i] <= 104`
- `edges.length == n - 1`
- `0 <= edges[i][0], edges[i][1] < n`
- `0 <= k <= 104`

comment：一开始没想到dp第二维数量很少：

// No need to increment beyond 15 because 10^4/2^15 =0. So anything beyond that will be zero

Solution:

```cpp
class Solution {
public:
    int maximumPoints(vector<vector<int>>& edges, vector<int>& coins, int k) {
        int n=coins.size();
        //build graph
        unordered_map<int,unordered_set<int>> g;
        for(auto& e: edges){
            int u=e[0], v=e[1];
            g[u].insert(v);
            g[v].insert(u);
        }
        //dp - max coin dfsto gather for subtree from root==u, divide div times
        vector<vector<int>> dp(n,vector<int>(15,-1)); //2^15>1e4
        //dfs - max coin to gather for subtree from root==u, parent=p, divide div times
        function<int(int,int,int)> dfs = [&](int p, int u, int div){
            if(dp[u][div]!=-1) return dp[u][div];
            int c=coins[u] / pow(2,div);
            //option 1:
            int cand1 = c-k;
            for(auto v: g[u]){
                if(v==p) continue;//dont go back
                //process u->v
                cand1+=dfs(u, v, div);
            }
            //option 2:
            int cand2 = c/2;
            for(auto v: g[u]){
                if(v==p) continue;//dont go back
                //process u->v
                if(div<14)//cutting branches
                    cand2+=dfs(u, v, div+1);
            }
            //res = max of op1 and op2
            dp[u][div]=max(cand1, cand2);
            return dp[u][div];
        };
        return dfs(-1,0,0);
    }
};

/*
n: 2~1e5
coin: 0~1e4
k: 0~1e4
================================
graph - tree dfs

f(node): max coin to tree with root==node
f(node,2^div) max= 
    1. coin-k + all f(node->child,div  )
    2. coin/2 + all f(node->child,div+1)

using dp[node,2^div]

init: when node==NULL, return 0

return: dp[root,0]
*/
```