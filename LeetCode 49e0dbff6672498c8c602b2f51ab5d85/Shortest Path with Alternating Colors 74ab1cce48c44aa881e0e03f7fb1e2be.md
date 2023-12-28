# Shortest Path with Alternating Colors

#: 1129
Difficult: Medium
Tags: BFS, Graph
link: https://leetcode.com/problems/shortest-path-with-alternating-colors/description/
Priority: Medium
Created time: June 28, 2023 12:23 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given an integer `n`, the number of nodes in a directed graph where the nodes are labeled from `0` to `n - 1`. Each edge is red or blue in this graph, and there could be self-edges and parallel edges.

You are given two arrays `redEdges` and `blueEdges` where:

- `redEdges[i] = [ai, bi]` indicates that there is a directed red edge from node `ai` to node `bi` in the graph, and
- `blueEdges[j] = [uj, vj]` indicates that there is a directed blue edge from node `uj` to node `vj` in the graph.

Return an array `answer` of length `n`, where each `answer[x]` is the length of the shortest path from node `0` to node `x` such that the edge colors alternate along the path, or `-1` if such a path does not exist.

**Example 1:**

```
Input: n = 3, redEdges = [[0,1],[1,2]], blueEdges = []
Output: [0,1,-1]

```

**Example 2:**

```
Input: n = 3, redEdges = [[0,1]], blueEdges = [[2,1]]
Output: [0,1,-1]

```

**Constraints:**

- `1 <= n <= 100`
- `0 <= redEdges.length, blueEdges.length <= 400`
- `redEdges[i].length == blueEdges[j].length == 2`
- `0 <= ai, bi, uj, vj < n`

comment: 思路简单，take sometime to implement, 注意corner case：visited要分红蓝

```cpp
class Solution {
public:
    vector<int> shortestAlternatingPaths(int n, vector<vector<int>>& redEdges, vector<vector<int>>& blueEdges) {
        vector<int> res(n,INT_MAX);
        unordered_map<int,unordered_set<int>> rg;
        unordered_map<int,unordered_set<int>> bg;
        for(auto& re: redEdges){
            int a=re[0], b=re[1];
            rg[a].insert(b);
        }
        for(auto& be: blueEdges){
            int a=be[0], b=be[1];
            bg[a].insert(b);
        }

        bfs(res,rg,bg,true);
        bfs(res,rg,bg,false);

        for(auto& x: res){ if(x==INT_MAX) x=-1; }
        return res;
    }

    void bfs(vector<int>& res, unordered_map<int,unordered_set<int>>& rg, unordered_map<int,unordered_set<int>>& bg, bool isInitRed){
        int n=res.size();
        queue<int> q;
        vector<vector<int>> visited(n,vector<int>(2,0));//redNxt1, blueNxt0, in case of use circle to change r/b req
        visited[0][isInitRed]=1;
        q.push(0);
        int dist=0;
        bool isRed=isInitRed;
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                int cur=q.front(); q.pop();
                res[cur]=min(res[cur],dist);
                auto& st=isRed?rg[cur]:bg[cur];
                for(int nxt: st){
                    if(visited[nxt][isRed]) continue;
                    visited[nxt][isRed]=1;
                    q.push(nxt);
                }
            }
            dist++;
            isRed=!isRed;
        }
    }
};

/*
node: 0~n-1
redEdges = [{a,b}]
blueEdges = [{a,b}]

directed graph
return [node 0 -> node x] for x: 0~n-1, color alternate, fallback=-1

*/
```