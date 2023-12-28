# Find the City With the Smallest Number of Neighbors

#: 1334
Difficult: Medium
Tags: BellmanFord, DP, Dijkstra, Floyd, Graph, SPFA, shortest_path
link: https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/description/
Priority: Medium
Status: More Solution, checkBetterSolution, toRevisit
Created time: June 28, 2023 12:22 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

There are `n` cities numbered from `0` to `n-1`. Given the array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is **at most** `distanceThreshold`, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities ***i*** and ***j*** is equal to the sum of the edges' weights along that path.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/01/16/find_the_city_01.png](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_01.png)

```
Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
Output: 3
Explanation:The figure above describes the graph.
The neighboring cities at a distanceThreshold = 4 for each city are:
City 0 -> [City 1, City 2]
City 1 -> [City 0, City 2, City 3]
City 2 -> [City 0, City 1, City 3]
City 3 -> [City 1, City 2]
Cities 0 and 3 have 2 neighboring cities at a distanceThreshold = 4, but we have to return city 3 since it has the greatest number.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/01/16/find_the_city_02.png](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_02.png)

```
Input: n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
Output: 0
Explanation:The figure above describes the graph.
The neighboring cities at a distanceThreshold = 2 for each city are:
City 0 -> [City 1]
City 1 -> [City 0, City 4]
City 2 -> [City 3, City 4]
City 3 -> [City 2, City 4]
City 4 -> [City 1, City 2, City 3]
The city 0 has 1 neighboring city at a distanceThreshold = 2.

```

**Constraints:**

- `2 <= n <= 100`
- `1 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 3`
- `0 <= fromi < toi < n`
- `1 <= weighti, distanceThreshold <= 10^4`
- All pairs `(fromi, toi)` are distinct.

comment: Runtime and Memory performance only beat 5%, check better solution

Solution:

for each node, do a dijkstra

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<unordered_map<int,int>> g(n);
        for(auto& e: edges){
            int u=e[0], v=e[1], w=e[2];
            g[u][v]=g[v][u]=w;
        }
        
        int res=-1, minCities=INT_MAX;
        for(int src=n-1; src>=0; src--){
            int cities=dijkstra(g, distanceThreshold, src);
            if(minCities>cities){
                minCities=cities;
                res=src;
            }
        }
        return res;
    }

    //return: reachable cities
    int dijkstra(vector<unordered_map<int,int>>& g, int distanceThreshold, int src){
        int n=g.size();
        priority_queue<ii,vector<ii>,greater<>> pq;
        vector<int> dist(n,INT_MAX);
        pq.push({0,src});//dist,node
        while(!pq.empty()){
            auto [d,u]=pq.top(); pq.pop();
            if(dist[u]==INT_MAX)
            dist[u]=d;
            for(auto [v,w]: g[u]){
                if(dist[v]!=INT_MAX) continue;
                if(d+w>distanceThreshold) continue;
                pq.push({d+w,v});
            }
        }

        int cnt=0;
        for(int d: dist){
            if(d!=INT_MAX) cnt++;
        }
        return cnt;
    }
};
```