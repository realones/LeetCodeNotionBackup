# Maximum Length of Pair Chain

#: 646
Difficult: Medium
Tags: Array, DFS, DP, Graph, Greedy, Sort
link: https://leetcode.com/problems/maximum-length-of-pair-chain/
Priority: Medium
Status: More Solution
Created time: August 26, 2023 3:45 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an array of `n` pairs `pairs` where `pairs[i] = [lefti, righti]` and `lefti < righti`.

A pair `p2 = [c, d]` **follows** a pair `p1 = [a, b]` if `b < c`. A **chain** of pairs can be formed in this fashion.

Return *the length longest chain which can be formed*.

You do not need to use up all the given intervals. You can select pairs in any order.

**Example 1:**

```
Input: pairs = [[1,2],[2,3],[3,4]]
Output: 2
Explanation: The longest chain is [1,2] -> [3,4].

```

**Example 2:**

```
Input: pairs = [[1,2],[7,8],[4,5]]
Output: 3
Explanation: The longest chain is [1,2] -> [4,5] -> [7,8].

```

**Constraints:**

- `n == pairs.length`
- `1 <= n <= 1000`
- `1000 <= lefti < righti <= 1000`

comment:

我的solution用的dfs+dp，但是tag里面没有dfs，而是有greedy和sort

```cpp
class Solution {
public:
    unordered_map<int,int> dp;//node-len
    int findLongestChain(vector<vector<int>>& pairs) {
        int n=pairs.size();
        //build graph
        unordered_map<int,unordered_set<int>> g;// from_tos
        unordered_map<int,int> ind;
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                if(i==j) continue;
                if(pairs[i][1]<pairs[j][0]) {
                    ind[j]++;
                    g[i].insert(j);
                }
            }
        }
        //
        int res=0;
        for(int i=0; i<n; i++){
            if(ind[i]==0){
                res=max(res,dfs(g,i));
            }
        }
        return res;
    }

    int dfs(unordered_map<int,unordered_set<int>>& g, int start){
        if(dp.count(start)) return dp[start];
        dp[start]=1;
        for(int nxt: g[start]){
            dp[start]=max(dp[start],dfs(g,nxt)+1);
        }
        return dp[start];
    }
};
/*
pairs=[{l,r}]

[a,b] ->(if b<c)-> [c,d] 

longest chain of pairs
================================
graph
build edges p1 -> p2

dp: p->len

for each ind == 0, dfs

*/
```