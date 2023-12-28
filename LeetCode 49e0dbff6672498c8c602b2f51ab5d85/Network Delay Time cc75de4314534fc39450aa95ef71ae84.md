# Network Delay Time

#: 743
Difficult: Medium
Tags: Dijkstra, Graph, shortest_path
link: https://leetcode.com/problems/network-delay-time/description/
Priority: Low
Created time: June 28, 2023 12:26 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return *the **minimum** time it takes for all the* `n` *nodes to receive the signal*. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

```
Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2

```

**Example 2:**

```
Input: times = [[1,2,1]], n = 2, k = 1
Output: 1

```

**Example 3:**

```
Input: times = [[1,2,1]], n = 2, k = 2
Output: -1

```

**Constraints:**

- `1 <= k <= n <= 100`
- `1 <= times.length <= 6000`
- `times[i].length == 3`
- `1 <= ui, vi <= n`
- `ui != vi`
- `0 <= wi <= 100`
- All the pairs `(ui, vi)` are **unique**. (i.e., no multiple edges.)

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        //build graph
        vector<unordered_map<int,int>> g(n+1);//to use vec
        for(auto& t: times){
            int u=t[0], v=t[1], w=t[2];
            g[u][v]=w;
        }

        priority_queue<ii,vector<ii>, greater<>> pq;//dist from k __ node
        vector<int> dist(n+1, INT_MAX);
        dist[0]=0;

        pq.push({0,k});
        while(!pq.empty()){
            auto [d,u] = pq.top(); pq.pop();
            if(dist[u]!=INT_MAX) continue;
            dist[u]=d;

            for(auto [v,w]:g[u]){
                if(dist[v]!=INT_MAX) continue;
                pq.push({d+w,v});
            }
        }

        int res=*max_element(dist.begin(), dist.end());
        return res==INT_MAX?-1:res;
    }
};

/*
dijkstra for spreading all from node k
*/
```