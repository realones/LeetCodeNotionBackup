# Path with Maximum Probability

#: 1514
Difficult: Medium
Tags: Array, BFS, Dijkstra, Graph, PQ, shortest_path
link: https://leetcode.com/problems/path-with-maximum-probability/description/
Priority: High
Status: Dig More
Created time: June 27, 2023 6:00 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an undirected weighted graph of `n` nodes (0-indexed), represented by an edge list where `edges[i] = [a, b]` is an undirected edge connecting the nodes `a` and `b` with a probability of success of traversing that edge `succProb[i]`.

Given two nodes `start` and `end`, find the path with the maximum probability of success to go from `start` to `end` and return its success probability.

If there is no path from `start` to `end`, **return 0**. Your answer will be accepted if it differs from the correct answer by at most **1e-5**.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/09/20/1558_ex1.png](https://assets.leetcode.com/uploads/2019/09/20/1558_ex1.png)

```
Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
Output: 0.25000
Explanation: There are two paths from start to end, one having a probability of success = 0.2 and the other has 0.5 * 0.5 = 0.25.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/09/20/1558_ex2.png](https://assets.leetcode.com/uploads/2019/09/20/1558_ex2.png)

```
Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.3], start = 0, end = 2
Output: 0.30000

```

**Example 3:**

![https://assets.leetcode.com/uploads/2019/09/20/1558_ex3.png](https://assets.leetcode.com/uploads/2019/09/20/1558_ex3.png)

```
Input: n = 3, edges = [[0,1]], succProb = [0.5], start = 0, end = 2
Output: 0.00000
Explanation: There is no path between 0 and 2.

```

**Constraints:**

- `2 <= n <= 10^4`
- `0 <= start, end < n`
- `start != end`
- `0 <= a, b < n`
- `a != b`
- `0 <= succProb.length == edges.length <= 2*10^4`
- `0 <= succProb[i] <= 1`
- There is at most one edge between every two nodes.

```cpp
using di=pair<double,int>;
class Solution {
public:
    //n:2~1e4, start/end valid, start!=end, a<b, succProb:0~1
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        //build weighted graph
        vector<unordered_map<int,double>> g(n);//u - [v,p]
        for(int i=0;i<edges.size();i++){
            int u=edges[i][0], v=edges[i][1];
            double p=succProb[i];
            g[u][v]=p;
            g[v][u]=p;
        }
        priority_queue<di> pq;//top is max p //p_from_start - node
        vector<double> prob(n,-1);//prob of each node from start
        
        pq.push({1,start});
        while(!pq.empty()){
            auto [sp, u] = pq.top(); pq.pop();
            if(prob[u]!=-1) continue;
            prob[u]=sp;

            for(auto [v,p]: g[u]){
                if(prob[v]!=-1) continue;
                pq.push({sp*p,v});
            }
        }

        return max(prob[end],0.0);
```