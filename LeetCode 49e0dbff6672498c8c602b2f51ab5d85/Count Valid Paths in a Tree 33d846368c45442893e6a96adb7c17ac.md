# Count Valid Paths in a Tree

#: 2867
Difficult: Hard
Tags: DFS, DP, Graph, Tree
link: https://leetcode.com/problems/count-valid-paths-in-a-tree/description/
Priority: High
Status: HardToImplement, No Idea At First, toRevisit, todo
Created time: September 25, 2023 5:27 PM
Last edited time: October 18, 2023 3:46 AM
practicedTimes: InProgress
source: LC_Contests
related: Minimum Edge Weight Equilibrium Queries in a Tree (Minimum%20Edge%20Weight%20Equilibrium%20Queries%20in%20a%20Tree%20ba9b8fbaf4f84f88b3432babdc3bf0fb.md)

There is an undirected tree with `n` nodes labeled from `1` to `n`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the tree.

Return *the **number of valid paths** in the tree*.

A path `(a, b)` is **valid** if there exists **exactly one** prime number among the node labels in the path from `a` to `b`.

**Note** that:

- The path `(a, b)` is a sequence of **distinct** nodes starting with node `a` and ending with node `b` such that every two adjacent nodes in the sequence share an edge in the tree.
- Path `(a, b)` and path `(b, a)` are considered the **same** and counted only **once**.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/08/27/example1.png](https://assets.leetcode.com/uploads/2023/08/27/example1.png)

```
Input: n = 5, edges = [[1,2],[1,3],[2,4],[2,5]]
Output: 4
Explanation: The pairs with exactly one prime number on the path between them are:
- (1, 2) since the path from 1 to 2 contains prime number 2.
- (1, 3) since the path from 1 to 3 contains prime number 3.
- (1, 4) since the path from 1 to 4 contains prime number 2.
- (2, 4) since the path from 2 to 4 contains prime number 2.
It can be shown that there are only 4 valid paths.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/08/27/example2.png](https://assets.leetcode.com/uploads/2023/08/27/example2.png)

```
Input: n = 6, edges = [[1,2],[1,3],[2,4],[3,5],[3,6]]
Output: 6
Explanation: The pairs with exactly one prime number on the path between them are:
- (1, 2) since the path from 1 to 2 contains prime number 2.
- (1, 3) since the path from 1 to 3 contains prime number 3.
- (1, 4) since the path from 1 to 4 contains prime number 2.
- (1, 6) since the path from 1 to 6 contains prime number 3.
- (2, 4) since the path from 2 to 4 contains prime number 2.
- (3, 6) since the path from 3 to 6 contains prime number 3.
It can be shown that there are only 6 valid paths.

```

**Constraints:**

- `1 <= n <= 105`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- The input is generated such that `edges` represent a valid tree.

Solution Graph → tree + dfs - **TLE**:

```
using ll=long long;
using pll=pair<long,long>;

class Solution {
public:
    long long countPaths(int n, vector<vector<int>>& edges) {//n:1~1e5, e:valid
        //build graph
        unordered_map<int,unordered_set<int>> g;
        for(auto& e: edges){
            int u=e[0], v=e[1];
            g[u].insert(v);
            g[v].insert(u);
        }
        //preprocess - find all prime nodes
        vector<bool> isPrime(n+1,false);
        for(int i=1; i<=n; i++){ isPrime[i]=isPrimeNum(i); }
        //process - dfs
        //input root, return cnt of 0,1
        int res=0;
        function<pll(int,int)> dfs = [&](int p, int u){
            int cnt0=0, cnt1=0;
            vector<vector<int>> cnts(2,vector<int>());
            for(auto v: g[u]){
                if(v==p) continue;//dont go back
                //process u->v
                auto [c0, c1] = dfs(u, v);
                cnts[0].push_back(c0);
                cnts[1].push_back(c1);
                cnt0+=c0, cnt1+=c1;
            }
            pll to_return;
            if(!isPrime[u]){
                //i:0, j:1
                for(int i=0;i<cnts[0].size();i++){
                    for(int j=i+1;j<cnts[0].size();j++){
                        res+=cnts[0][i]*cnts[1][j];
                    }
                }
                //i:1, j:0
                for(int i=0;i<cnts[0].size();i++){
                    for(int j=i+1;j<cnts[0].size();j++){
                        res+=cnts[1][i]*cnts[0][j];
                    }
                }
                //using root
                for(int i=0;i<cnts[0].size();i++){
                    res+=cnts[1][i];
                }
                to_return={cnt0+1,cnt1};
            }
            else{
                //i:0, j:0
                for(int i=0;i<cnts[0].size();i++){
                    for(int j=i+1;j<cnts[0].size();j++){
                        res+=cnts[0][i]*cnts[0][j];
                    }
                }
                //using root
                for(int i=0;i<cnts[0].size();i++){
                    res+=cnts[0][i];
                }
                to_return={0,cnt0+1};
            }
            return to_return;
        };
        dfs(-1,1);
        return res;
    }

    bool isPrimeNum(int n) {
        if (n <= 1)  return false;
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) { return false; }
        }
        return true; // n is prime
    }
};

/*
valid path (a,b): exactly one *prime number* among the nodes in path (a,b) -- 1 based
return: cnt of valid paths
================================

preprocess to find all prime nodes,

================================
lca
(a,b) = (lca,a) + (lca,b)
====>
dfs, for each root, find l, r (can be root itself), then l->root->r
based on root is not prime:0 or prime:1
dfs1(r): how many path to it is 0
dfs2(r): how many path to it is 1

*/
```