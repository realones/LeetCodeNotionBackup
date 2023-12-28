# Sum of Distances in Tree

#: 834
Difficult: Hard
Tags: DFS, DP, Graph, ReRoot, Tree
link: https://leetcode.com/problems/sum-of-distances-in-tree/description/
Priority: High
Status: Dig More, No Idea At First, toRevisit
Created time: December 20, 2023 12:28 AM
Last edited time: December 20, 2023 10:42 AM
source: googleFreq

There is an undirected connected tree with `n` nodes labeled from `0` to `n - 1` and `n - 1` edges.

You are given the integer `n` and the array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

Return an array `answer` of length `n` where `answer[i]` is the sum of the distances between the `ith` node in the tree and all other nodes.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg)

```
Input: n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
Output: [8,12,6,10,10,10]
Explanation: The tree is shown above.
We can see that dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5)
equals 1 + 1 + 2 + 2 + 2 = 8.
Hence, answer[0] = 8, and so on.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist2.jpg](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist2.jpg)

```
Input: n = 1, edges = []
Output: [0]

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist3.jpg](https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist3.jpg)

```
Input: n = 2, edges = [[1,0]]
Output: [1,1]

```

**Constraints:**

- `1 <= n <= 3 * 104`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- The given input represents a valid tree.

```cpp
//lee215

class Solution {
public:
    unordered_map<int,unordered_set<int>> g;
    vector<int> count;
    vector<int> res;

    vector<int> sumOfDistancesInTree(int n, vector<vector<int>>& edges) {
        res.resize(n,0);
        count.resize(n,1);
        //build graph
        for(auto& e: edges){
            int u=e[0], v=e[1];
            g[u].insert(v);
            g[v].insert(u);
        }
        dfs(0,-1);
        dfs2(0,-1);
        return res;
    }

    void dfs(int u, int p){
        for(auto v: g[u]){
            if(v==p) continue;
            dfs(v,u);
            count[u] += count[v];
            res[u] += res[v] + count[v];
        }
    }

    void dfs2(int u, int p){
        for(auto v: g[u]){
            if(v==p) continue;
            res[v] = res[u] - count[v] + count.size() - count[v];
            dfs2(v,u);
        }
    }
};

/*
ReRoot:
    idea: 0 as root, then recursively treat sub nodes as root

0 as root

build graph: mp<u,st<v>> g
cnt[u]: nodes cnt in subtree[u]
    = sum(cnt[v]): post order dfs1 (bottom-up)
res[u]: result for each node
thought1
    res1[u]: the sum of dist from all subTree nodes
        res[u] = sum(res[v] + cnt[v])
            : post order dfs1 (bottom-up)
    +
    res2[u]: the sum of dist from all non subTree nodes
            res[p]: 
            -
            (res[u]+cnt[u]): contribute from subTree u to p
            +
            n-cnt[u]: sum of cnts of non subTree u, each to_node dist++
                : pre order dfs2 (top-down)
        ==>
            res[p]-cnt[u]
            +
            n-cnt[u]
thought 2(easier):
    step1: the sum of dist from all subTree nodes
        res[u] = sum(res[v] + cnt[v])
            : post order dfs1 (bottom-up)
    step2: the sum of dist from all other nodes
        on node u, for each v
        res[v] =
            res[u]
            -cnt[v]: go from u to v, for subtree[v] nodes, each to_node dist--
            +n-cnt[v]: sum of cnts of non subTree v, each to_node dist++
                : pre order dfs2 (top-down)
        ==>
            res[p]-cnt[u]
            +
            n-cnt[u]

post order dfs traverse, update count and res(step1)
    count[u] = sum(count[v]) + 1
    res[u] = sum(res[v]) + sum(count[v])

pre order dfs traverse, update res
    move u->v:
        count[v] get 1 closer to cur
        n-count[v] get 1 farther to cur
    res[v] = res[u] - count[v] + n - count[v]
================================
graph tree dfs

dfs postOrder = recursion = bottom-up
dfs preOrder  = top-down

*/
```