# Cheapest Flights Within K Stops

#: 787
Difficult: Medium
Tags: BFS, BellmanFord, DFS, DP, Dijkstra, Graph, PQ, shortest_path
link: https://leetcode.com/problems/cheapest-flights-within-k-stops/description/
Priority: High
Status: CalcuComplexity, Dig More, More Solution, toSummarize
Created time: June 28, 2023 12:12 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return ***the cheapest price** from* `src` *to* `dst` *with at most* `k` *stops.* If there is no such route, return **`-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-3drawio.png](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-3drawio.png)

```
Input: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
Output: 700
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities [0,1,2,3] is cheaper but is invalid because it uses 2 stops.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-1drawio.png](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-1drawio.png)

```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
Explanation:
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 2 is marked in red and has cost 100 + 100 = 200.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-2drawio.png](https://assets.leetcode.com/uploads/2022/03/18/cheapest-flights-within-k-stops-2drawio.png)

```
Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
Output: 500
Explanation:
The graph is shown above.
The optimal path with no stops from city 0 to 2 is marked in red and has cost 500.

```

**Constraints:**

- `1 <= n <= 100`
- `0 <= flights.length <= (n * (n - 1) / 2)`
- `flights[i].length == 3`
- `0 <= fromi, toi < n`
- `fromi != toi`
- `1 <= pricei <= 104`
- There will not be any multiple flights between two cities.
- `0 <= src, dst, k < n`
- `src != dst`

comment: learned Bellman Ford, but the dijkstra version solution is hard to understand

Solution - Bellman Ford

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<int> dist(n, INT_MAX);
        dist[src] = 0;

        //at most n-1 edges, edge_cnt==0 already initiated
        for (int i = 1; i <= k+1; i++) {
            vector<int> tmp(dist);//improved by rolling arr
            //bool hasChange=false; // flag to check if update occurred, just improvement

            for (auto& e : flights) {
                int u = e[0], v = e[1], w = e[2]; // Corrected typo here
                if (dist[u] != INT_MAX && dist[u] + w < tmp[v]) {
                    tmp[v] = dist[u] + w;
                    //hasChange=true;
                }
            }
            //if(!hasChange) break; // No update occurred, break early
            dist=tmp;
        }
        return dist[dst]==INT_MAX?-1:dist[dst];
    }
};

/*
solution 1: BFS with cutting edges of "price + distance >= dist[neighbour]"

solution 2: Bellman Ford
*/
```

Solution - Dijkstra

need to dig more, using “steps” arr instead of “dists” arr

```cpp
using iii=tuple<int,int,int>;//node,dist,step

class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& edges, int src, int dst, int k) {
        vector<vector<pair<int, int>>> adj(n);
        for (auto& e : edges) {
            int u=e[0], v=e[1], w=e[2];
            adj[u].push_back({v, w});
        }
        vector<int> steps(n, INT_MAX);
        
        priority_queue<iii, vector<iii>, greater<>> pq;
        // {dist_from_src_node, node, number_of_steps_from_src_node}
        pq.push({0, src, 0});

        while (!pq.empty()) {
            auto [d,u,s] = pq.top(); pq.pop();
            // We have already encountered a path with a lower cost and fewer stops,
            if (s > steps[u]) continue;
            steps[u] = s;
            if (u == dst) return d;
            for (auto& [v, w] : adj[u]) {
                if(s + 1 > k + 1) continue;//the number of steps exceeds the limit.
                pq.push({d + w, v, s + 1});
            }
        }
        return -1;
    }
};
```