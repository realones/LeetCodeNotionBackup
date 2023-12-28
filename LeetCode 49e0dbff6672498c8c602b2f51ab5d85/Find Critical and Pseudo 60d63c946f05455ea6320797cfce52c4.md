# Find Critical and Pseudo

#: 1489
Difficult: Hard
Tags: Graph, MST(Minimum Spanning Tree), Strongly Connected Component, Tree
link: https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/description/
Priority: High
Status: Dig More, No Idea At First, toRevisit
Created time: June 27, 2023 11:44 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given a weighted undirected connected graph with `n` vertices numbered from `0` to `n - 1`, and an array `edges` where `edges[i] = [ai, bi, weighti]` represents a bidirectional and weighted edge between nodes `ai` and `bi`. A minimum spanning tree (MST) is a subset of the graph's edges that connects all vertices without cycles and with the minimum possible total edge weight.

Find *all the critical and pseudo-critical edges in the given graph's minimum spanning tree (MST)*. An MST edge whose deletion from the graph would cause the MST weight to increase is called a *critical edge*. On the other hand, a pseudo-critical edge is that which can appear in some MSTs but not all.

Note that you can return the indices of the edges in any order.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/06/04/ex1.png](https://assets.leetcode.com/uploads/2020/06/04/ex1.png)

```
Input: n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]
Output: [[0,1],[2,3,4,5]]
Explanation: The figure above describes the graph.
The following figure shows all the possible MSTs:

Notice that the two edges 0 and 1 appear in all MSTs, therefore they are critical edges, so we return them in the first list of the output.
The edges 2, 3, 4, and 5 are only part of some MSTs, therefore they are considered pseudo-critical edges. We add them to the second list of the output.

```

![https://assets.leetcode.com/uploads/2020/06/04/msts.png](https://assets.leetcode.com/uploads/2020/06/04/msts.png)

**Example 2:**

![https://assets.leetcode.com/uploads/2020/06/04/ex2.png](https://assets.leetcode.com/uploads/2020/06/04/ex2.png)

```
Input: n = 4, edges = [[0,1,1],[1,2,1],[2,3,1],[0,3,1]]
Output: [[],[0,1,2,3]]
Explanation: We can observe that since all 4 edges have equal weight, choosing any 3 edges from the given 4 will yield an MST. Therefore all 4 edges are pseudo-critical.

```

**Constraints:**

- `2 <= n <= 100`
- `1 <= edges.length <= min(200, n * (n - 1) / 2)`
- `edges[i].length == 3`
- `0 <= ai < bi < n`
- `1 <= weighti <= 1000`
- All pairs `(ai, bi)` are **distinct**.

================================

comment: new algorithm that i don’t know, check following answers:

wisdompeak:

[https://www.youtube.com/watch?v=ozlYJy-2ZEY](https://www.youtube.com/watch?v=ozlYJy-2ZEY)

huahua：

[https://www.youtube.com/watch?v=GzPUvV85kBI](https://www.youtube.com/watch?v=GzPUvV85kBI)

MST:

[https://www.youtube.com/watch?v=wmW8G8SrXDs](https://www.youtube.com/watch?v=wmW8G8SrXDs)

================================

```cpp
using ii=pair<int,int>;

class Solution {
public:
    vector<vector<int>> findCriticalAndPseudoCriticalEdges(int n, vector<vector<int>>& edges) {
        //preprocess edges sort by w. -  O(ElogE)
        vector<vector<int>> q;
        for(int i=0;i<edges.size();i++){
            int id=i, u=edges[i][0], v=edges[i][1], w=edges[i][2];
            q.push_back({w,u,v,id});
        }
        sort(q.begin(), q.end());

        //get min_cost of mst
        int min_cost = MST(n, q, -1, -1);

        vector<int> criticals, pseudos;
        for(int i=0;i<q.size();i++){
            //exclude it to find critical
            if(MST(n,q,i,-1) > min_cost) criticals.push_back(q[i][3]);
            //include it to find pseudo-critical
            else if(MST(n,q,-1,i) == min_cost) pseudos.push_back(q[i][3]);
        }
        return {criticals, pseudos};
    }

    //edges: [u,v,w]
    int MST(int n, vector<vector<int>>& q, int ex, int in){
        //union find
        vector<int> p(n);//parent
        iota(p.begin(), p.end(),0);//init

        function<int(int)> find = [&](int x){
            return x==p[x] ? x : p[x]=find(p[p[x]]);
        };

        //process
        int cost=0;
        int component=n;

        if(in>=0){
            auto& e=q[in];
            int id=e[3], u=e[1], v=e[2], w=e[0];
            p[u]=v;
            cost += w;
            component--;
        }

        for(int i=0;i<q.size();i++){
            if(i==ex) continue;
            auto& e = q[i];
            int id=e[3], u=e[1], v=e[2], w=e[0];
            int ru = find(u), rv = find(v);//find roots of u,v
            if(ru==rv) continue; //if connected
            p[ru]=rv;//union
            cost+=w;
            component--;
        }
        if(component>1) return INT_MAX;
        return cost;
    }
};

/*
edges.sz<=200: brute force each edge

critical edge:
1. delete will cause MST inc
2. cut edge

pseudo critical edge:
1. force put the edge into MST and try if MST inc

================================
impl
1. record edge id
2. sort edges by weight
3. build a mst to get min_cost
4. for each edge:
    a. exclude it to check if critical
    b. include it to check if pseudo-critical
================================
T: O(ElogE) + O(E^2)
S: O(V)
*/
```