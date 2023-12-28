# Find Number of Coins to Place in Tree Nodes

#: 2973
Difficult: Hard
Tags: DFS, Graph, Tree, recursion
link: https://leetcode.com/problems/find-number-of-coins-to-place-in-tree-nodes/
Priority: Medium
Status: HardToImplement
Created time: December 25, 2023 12:24 AM
Last edited time: December 25, 2023 12:25 AM
source: googleFreq

You are given an **undirected** tree with `n` nodes labeled from `0` to `n - 1`, and rooted at node `0`. You are given a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

You are also given a **0-indexed** integer array `cost` of length `n`, where `cost[i]` is the **cost** assigned to the `ith` node.

You need to place some coins on every node of the tree. The number of coins to be placed at node `i` can be calculated as:

- If size of the subtree of node `i` is less than `3`, place `1` coin.
- Otherwise, place an amount of coins equal to the **maximum** product of cost values assigned to `3` distinct nodes in the subtree of node `i`. If this product is **negative**, place `0` coins.

Return *an array* `coin` *of size* `n` *such that* `coin[i]` *is the number of coins placed at node* `i`*.*

**Example 1:**

![https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012641.png](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012641.png)

```
Input: edges = [[0,1],[0,2],[0,3],[0,4],[0,5]], cost = [1,2,3,4,5,6]
Output: [120,1,1,1,1,1]
Explanation: For node 0 place 6 * 5 * 4 = 120 coins. All other nodes are leaves with subtree of size 1, place 1 coin on each of them.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012614.png](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012614.png)

```
Input: edges = [[0,1],[0,2],[1,3],[1,4],[1,5],[2,6],[2,7],[2,8]], cost = [1,4,2,3,5,7,8,-4,2]
Output: [280,140,32,1,1,1,1,1,1]
Explanation: The coins placed on each node are:
- Place 8 * 7 * 5 = 280 coins on node 0.
- Place 7 * 5 * 4 = 140 coins on node 1.
- Place 8 * 2 * 2 = 32 coins on node 2.
- All other nodes are leaves with subtree of size 1, place 1 coin on each of them.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012513.png](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012513.png)

```
Input: edges = [[0,1],[0,2]], cost = [1,2,-2]
Output: [0,1,1]
Explanation: Node 1 and 2 are leaves with subtree of size 1, place 1 coin on each of them. For node 0 the only possible product of cost is 2 * 1 * -2 = -4. Hence place 0 coins on node 0.

```

**Constraints:**

- `2 <= n <= 2 * 104`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `cost.length == n`
- `1 <= |cost[i]| <= 104`
- The input is generated such that `edges` represents a valid tree.

```cpp
//cp from https://leetcode.com/problems/find-number-of-coins-to-place-in-tree-nodes/solutions/4447466/dfs-track-largest-3-smallest-2-from-each-sub-tree-very-simple-and-easy-to-understand/

using ll=long long;
class Solution {
public:
    vector<ll> ans;

    vector<ll> placedCoins(vector<vector<int>>& edges, vector<int>& cost) {
        int n=cost.size();
        ans.resize(n, 0); 
        vector<vector<int>> g(n);
        for(auto e : edges){ g[e[0]].push_back(e[1]); g[e[1]].push_back(e[0]); }
        dfs(g, cost, 0, -1);
        return ans;
    }

    vector<ll> dfs(vector<vector<int>>& g, vector<int>& cost, int u, int p){
        vector<ll> usefulCost = {cost[u]};
        for(auto v: g[u]){
            if(v == p) continue;
            vector<ll> vec = dfs(g, cost, v, u);  
            for(auto e: vec) usefulCost.push_back(e);  
        }
        sort(usefulCost.begin(), usefulCost.end(), greater<ll>());
        ll sz = usefulCost.size();
        
        // check if the size of the sub tree less than 3 then set cost to 1 and return from here.
        if(usefulCost.size() < 3) { ans[u] = 1; return usefulCost; }  
        // check if the product of the 2nd larget and 3rd larget is greater than smallest two numbers 
        if(usefulCost[1] * usefulCost[2] > usefulCost[sz - 1] * usefulCost[sz -2]) {   
            ans[u] = usefulCost[0] * usefulCost[1] * usefulCost[2];  
        }
        else {
            ans[u] = usefulCost[0] * usefulCost[sz - 1] * usefulCost[sz - 2];   
        }
        if(ans[u] < 0) ans[u] = 0;

        //return largest 3 and smallest two items, only those can be useful in later on step and discard others.
        if(usefulCost.size() <= 5) return usefulCost;
        return {usefulCost[0], usefulCost[1], usefulCost[2], usefulCost[sz-2], usefulCost[sz-1]};   
    }
};

/*
bottom up recursion - post order dfs

each node:
    cnt = accumulate childCnt
    child vals = accumulate childVals (when return, + itself)
    calcu coin needed:
        if(cnt<3) 1 coin
        else
            max product of 3 child vals
            coin = max(prod,0)
enhancement:
    not pass all childVals, but useful ones
        if all positive, only need largest 3
        if all negtive, only need largest 3 (not required, since have to >=0)
        if both pos and neg, need 2 neg, 1 pos, or 3 pos
    so:
        pos: largest 3
        neg: smallest 2
*/
```