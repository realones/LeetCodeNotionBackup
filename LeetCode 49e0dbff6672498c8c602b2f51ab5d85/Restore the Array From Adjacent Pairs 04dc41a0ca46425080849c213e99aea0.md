# Restore the Array From Adjacent Pairs

#: 1743
Difficult: Medium
Tags: Array, Graph, Hash
link: https://leetcode.com/problems/restore-the-array-from-adjacent-pairs
Priority: Low
Created time: November 11, 2023 2:40 AM
Last edited time: November 11, 2023 2:41 AM
source: leetcode_daily

There is an integer array `nums` that consists of `n` **unique** elements, but you have forgotten it. However, you do remember every pair of adjacent elements in `nums`.

You are given a 2D integer array `adjacentPairs` of size `n - 1` where each `adjacentPairs[i] = [ui, vi]` indicates that the elements `ui` and `vi` are adjacent in `nums`.

It is guaranteed that every adjacent pair of elements `nums[i]` and `nums[i+1]` will exist in `adjacentPairs`, either as `[nums[i], nums[i+1]]` or `[nums[i+1], nums[i]]`. The pairs can appear **in any order**.

Return *the original array* `nums`*. If there are multiple solutions, return **any of them***.

**Example 1:**

```
Input: adjacentPairs = [[2,1],[3,4],[3,2]]
Output: [1,2,3,4]
Explanation: This array has all its adjacent pairs in adjacentPairs.
Notice that adjacentPairs[i] may not be in left-to-right order.

```

**Example 2:**

```
Input: adjacentPairs = [[4,-2],[1,4],[-3,1]]
Output: [-2,4,1,-3]
Explanation: There can be negative numbers.
Another solution is [-3,1,4,-2], which would also be accepted.

```

**Example 3:**

```
Input: adjacentPairs = [[100000,-100000]]
Output: [100000,-100000]

```

**Constraints:**

- `nums.length == n`
- `adjacentPairs.length == n - 1`
- `adjacentPairs[i].length == 2`
- `2 <= n <= 105`
- `105 <= nums[i], ui, vi <= 105`
- There exists some `nums` that has `adjacentPairs` as its pairs.

```cpp
class Solution {
public:
    vector<int> restoreArray(vector<vector<int>>& adjacentPairs) {
        //build graph and seen
        unordered_map<int, unordered_set<int>> g;
        unordered_map<int,int> mp;//elem_seenCnt
        for(auto& adj: adjacentPairs){
            int u=adj[0], v=adj[1];
            g[u].insert(v);
            g[v].insert(u);
            mp[u]++, mp[v]++;
        }

        //find start
        int start=-1;//assume input always valid
        for(auto[elem, cnt]: mp){
            if(cnt==1) start=elem;
        }

        //process
        vector<int> res;
        int u=start;
        int n=adjacentPairs.size()+1;
        for(int i=0; i<n; i++){
            res.push_back(u);
            int v=g[u].empty()?-1:*g[u].begin();
            g[u].erase(v);
            g[v].erase(u);
            u=v;
        }
        return res;
    }
};
/*
select any elem that only appear once as start, then go forward
graph build one line to draw all nodes
*
```