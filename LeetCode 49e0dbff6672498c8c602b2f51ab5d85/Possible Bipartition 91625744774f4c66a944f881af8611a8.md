# Possible Bipartition

#: 886
Difficult: Medium
Tags: BFS, Bipartition_graphColoring, Graph
link: https://leetcode.com/problems/possible-bipartition/
Priority: High
Status: More Solution, No Idea At First, toSummarize
Created time: June 28, 2023 12:24 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Is Graph Bipartite? (Is%20Graph%20Bipartite%2045c1135455a441f6b56570aee649ac62.md), Flower Planting With No Adjacent (Flower%20Planting%20With%20No%20Adjacent%20fee61ff1b1164d9eb2b6922355e3e5d8.md)

We want to split a group of `n` people (labeled from `1` to `n`) into two groups of **any size**. Each person may dislike some other people, and they should not go into the same group.

Given the integer `n` and the array `dislikes` where `dislikes[i] = [ai, bi]` indicates that the person labeled `ai` does not like the person labeled `bi`, return `true` *if it is possible to split everyone into two groups in this way*.

**Example 1:**

```
Input: n = 4, dislikes = [[1,2],[1,3],[2,4]]
Output: true
Explanation: The first group has [1,4], and the second group has [2,3].

```

**Example 2:**

```
Input: n = 3, dislikes = [[1,2],[1,3],[2,3]]
Output: false
Explanation: We need at least 3 groups to divide them. We cannot put them in two groups.

```

**Constraints:**

- `1 <= n <= 2000`
- `0 <= dislikes.length <= 104`
- `dislikes[i].length == 2`
- `1 <= ai < bi <= n`
- All the pairs of `dislikes` are **unique**.

comment: 

very similar to “Is Graph Bipartite”, 思路在此基础上又有点新颖

虽然靠自己想出来了，但是稍微花了点时间，因为刚刚做过上面的题，所以不难，mark priority as High, 因为思路蛮巧妙的这道题。

toSummarize:

there is no cicles of odd size == Bipartition

```cpp
class Solution {
public:
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        //build dislike graph
        unordered_map<int,unordered_set<int>> g;//to improve with vector
        for(auto& dislike: dislikes){
            int a=dislike[0], b=dislike[1];
            g[a].insert(b);
            g[b].insert(a);
        }
        //bfs to check if conflict
        
        unordered_map<int,int> mp;//node_ belong_to_st:1or-1

        for(int i=1; i<=n; i++){
            if(mp.count(i)) continue;
            if(!bfs(g,mp,i)) return false;
        }
        return true;
    }

    bool bfs(unordered_map<int,unordered_set<int>>& g, unordered_map<int,int>& mp, int start){
        queue<int> q;
        mp[start]=1;
        q.push(start);

        while(!q.empty()){
            int cur=q.front(); q.pop();
            //process
            for(auto nei: g[cur]){
                if(mp.count(nei)) {
                    if(mp[cur]*mp[nei]==1) return false;
                    continue;
                }
                mp[nei]=mp[cur]*(-1);
                q.push(nei);
            }
        }
        return true;
    }
};

/*
n people
dislikes -> likes? no?

if dislike put to
- group1
- group2

make dislike mutual - bidirection
remove ones nobody dislike/ dislike nobody - same thing

make sure there is no cicles of odd size

topSort - what if two cicles with odd size, not good
unionFind?
bfs/dfs - for each node, put into A or B, check if there will be conflict
    since bfs and dfs if 1 direction, from root to end, there won't be 2 start root for one 

conflict:
    if dist odd -> should be in same group
    if dist even -> should be in diff group
    ==》
    as long as the next nei is not in same group, it is fine

*/
```