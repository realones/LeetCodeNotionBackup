# Modify Graph Edge Weights

#: 2699
Difficult: Hard
Tags: Dijkstra, Graph, Greedy
link: https://leetcode.com/problems/modify-graph-edge-weights/
Priority: High
Status: Dig More, More Solution, No Idea At First
Created time: May 21, 2023 10:59 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given an **undirected weighted** **connected** graph containing `n` nodes labeled from `0` to `n - 1`, and an integer array `edges` where `edges[i] = [ai, bi, wi]` indicates that there is an edge between nodes `ai` and `bi` with weight `wi`.

Some edges have a weight of `-1` (`wi = -1`), while others have a **positive** weight (`wi > 0`).

Your task is to modify **all edges** with a weight of `-1` by assigning them **positive integer values** in the range `[1, 2 * 109]` so that the **shortest distance** between the nodes `source` and `destination` becomes equal to an integer `target`. If there are **multiple** **modifications** that make the shortest distance between `source` and `destination` equal to `target`, any of them will be considered correct.

Return *an array containing all edges (even unmodified ones) in any order if it is possible to make the shortest distance from* `source` *to* `destination` *equal to* `target`*, or an **empty array** if it's impossible.*

**Note:** You are not allowed to modify the weights of edges with initial positive weights.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/04/18/graph.png](https://assets.leetcode.com/uploads/2023/04/18/graph.png)

```
Input: n = 5, edges = [[4,1,-1],[2,0,-1],[0,3,-1],[4,3,-1]], source = 0, destination = 1, target = 5
Output: [[4,1,1],[2,0,1],[0,3,3],[4,3,1]]
Explanation: The graph above shows a possible modification to the edges, making the distance from 0 to 1 equal to 5.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/04/18/graph-2.png](https://assets.leetcode.com/uploads/2023/04/18/graph-2.png)

```
Input: n = 3, edges = [[0,1,-1],[0,2,5]], source = 0, destination = 2, target = 6
Output: []
Explanation: The graph above contains the initial edges. It is not possible to make the distance from 0 to 2 equal to 6 by modifying the edge with weight -1. So, an empty array is returned.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2023/04/19/graph-3.png](https://assets.leetcode.com/uploads/2023/04/19/graph-3.png)

```
Input: n = 4, edges = [[1,0,4],[1,2,3],[2,3,5],[0,3,-1]], source = 0, destination = 2, target = 6
Output: [[1,0,4],[1,2,3],[2,3,5],[0,3,1]]
Explanation: The graph above shows a modified graph having the shortest distance from 0 to 2 as 6.

```

**Constraints:**

- `1 <= n <= 100`
- `1 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 3`
- `0 <= ai, bi < n`
- `wi = -1` or `1 <= wi <= 107`
- `ai != bi`
- `0 <= source, destination < n`
- `source != destination`
- `1 <= target <= 109`
- The graph is connected, and there are no self-loops or repeated edges

Solution 1: Dijkstra

```cpp
/*
greedy
find dists of des -> nodes
find dists of sou -> nodes
    if shortest path contains u->v, w==-1, then
        //if shortest dist with u->v < target -> update u->v to target-s_u-d-v
        //if shortest dist with u->v = target -> update u->v = 1
        //if shortest dist with u->v > target -> update u->v = 1, if use this path, fail anyway
*/
using ii = pair<int,int>;
class Solution {
public:
    vector<vector<int>> modifiedGraphEdges(int n, vector<vector<int>>& edges, int source, int destination, int target) 
    {
        //prepare graph
        vector<unordered_map<int,int>> g(n);//u - [v,w]
        for (auto& e: edges)
        {
            int a = e[0], b = e[1], w=e[2];
            g[a][b] = w;
            g[b][a] = w;
        }
        
        //des -> nodes
        priority_queue<ii, vector<ii>, greater<>>pq;
        vector<int>distd(n, INT_MAX);

        pq.push({0, destination});
        while (!pq.empty())
        {
            auto [d, u] = pq.top(); pq.pop();
            if (distd[u]!=INT_MAX) continue;
            distd[u] = d;
            
            for (auto [v, w]: g[u])
            {
                if (distd[v]!=INT_MAX) continue;
                pq.push({d+abs(w), v});
            }
        }
            
        //sou -> nodes
        vector<int>dists(n, INT_MAX);
        pq.push({0, source});
        while (!pq.empty())
        {
            auto [d, u] = pq.top(); pq.pop();
            if (dists[u]!=INT_MAX) continue;
            dists[u] = d;
            if (u==destination && d != target) return {};
            for (auto [v, w]: g[u])
            {
                if (dists[v]!=INT_MAX) continue;
                if (g[u][v]==-1){
                    //if shortest dist with u->v < target -> update u->v to target-s_u-d-v
                    //if shortest dist with u->v = target -> update u->v = 1
                    //if shortest dist with u->v > target -> update u->v = 1, if use this path, fail anyway
                    if(dists[u]+abs(w)+distd[v]<target){
                        w = target-dists[u]-distd[v];
                        if(w<1) return {};
                        g[u][v] = w;
                        g[v][u] = w;                    
                    }
                    else{
                        g[u][v] = 1;
                        g[v][u] = 1;                    
                    }
                }
                pq.push({d+abs(w), v});
            }
        }

        for (auto& edge: edges)
        {
            int a = edge[0], b = edge[1];            
            edge[2] = g[a][b];
        }

        return edges;
    }
};
```

**Failed bfs solution**:

```cpp
/*
failed test case:
5
[[1,4,1],[2,4,-1],[3,0,2],[0,4,-1],[1,3,10],[1,0,10]]
0
2
15
*/

class Solution {
public:
    vector<vector<int>> modifiedGraphEdges(int n, vector<vector<int>>& edges, int source, int destination, int target) {
        //build weight matrix and graph with edges
        vector<vector<int>> weight(n,vector<int>(n,0));
        vector<unordered_set<int>> g(n,unordered_set<int>());
        for(auto& e: edges){
            int a=e[0],b=e[1],w=e[2];
            weight[a][b]=w;
            weight[b][a]=w;
            g[a].insert(b);
            g[b].insert(a);
        }
        //init total min weight from source
        //pair <min totalW from source, set of -1 edges>
        vector<pair<int,unordered_set<string>>> dist(n,{1e9,{}}); 
        dist[source]={0,{}};
        //bfs to get shortest dist from source to everywhere
        queue<int> q;
        q.push(source);
        while(!q.empty()){
            int cur=q.front(); q.pop();
            for(auto nxt: g[cur]){
                int w=weight[cur][nxt]; //cur->nxt
                auto [curW,specialEdges]=dist[cur]; //cur+nxt
                if(w==-1) {
                    w=1;
                    specialEdges.insert(to_string(cur)+"_"+to_string(nxt));
                    specialEdges.insert(to_string(nxt)+"_"+to_string(cur));
                }
                int totalW=curW+w;
                if(totalW<dist[nxt].first){
                    dist[nxt]={totalW,specialEdges};
                    q.push(nxt);
                }
                else if(totalW==dist[nxt].first){
                    dist[nxt]={totalW,specialEdges};
                    q.push(nxt);
                }
            }    
        }
        
        //====================
        //init total min weight from source with out -1 edges
        vector<int> distNoSpecialEdges(n,1e9); 
        distNoSpecialEdges[source]=0;
        //bfs to get shortest dist from source to everywhere
        q.push(source);
        while(!q.empty()){
            int cur=q.front(); q.pop();
            for(auto nxt: g[cur]){
                int w=weight[cur][nxt]; //cur->nxt
                if(w==-1) {continue;}
                int curW=distNoSpecialEdges[cur]; //cur+nxt
                int totalW=curW+w;
                if(totalW<distNoSpecialEdges[nxt]){
                    distNoSpecialEdges[nxt]=totalW;
                    q.push(nxt);
                }
            }    
        }
        if(distNoSpecialEdges[destination]<target) return {};
        //====================
        
        //handle edges
        auto [totalW, specialEdges]=dist[destination];
        if(specialEdges.empty() && totalW!=target) return {}; //corner cases
        if(!specialEdges.empty() && totalW>target) return {}; //corner cases
        
        for(auto &e: edges){
            int &a=e[0], &b=e[1], &w=e[2];
            if(w==-1){
                string key1=to_string(a)+"_"+to_string(b);
                string key2=to_string(b)+"_"+to_string(a);
                if( specialEdges.count(key1) || specialEdges.count(key2) ){
                    w=1;
                    if(specialEdges.size()==2){ 
                        w+=target-totalW;
                    }
                    if(specialEdges.count(key1)) specialEdges.erase(key1);
                    if(specialEdges.count(key2)) specialEdges.erase(key2);
                }
                else{
                    w=1e7;
                }
            }
        }
        return edges;
    }
};
```