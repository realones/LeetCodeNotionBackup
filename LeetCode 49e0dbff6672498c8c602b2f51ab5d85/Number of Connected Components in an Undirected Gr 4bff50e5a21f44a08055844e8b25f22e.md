# Number of Connected Components in an Undirected Graph

#: 323
Difficult: Medium
Tags: BFS, DFS, Graph, Union Find
link: https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/
Priority: Low
Status: More Solution
Created time: July 26, 2022 5:37 PM
Last edited time: October 16, 2023 1:02 AM
practicedTimes: 1
source: jiuzhang, 算初BFS

You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between `ai` and `bi` in the graph.

Return *the number of connected components in the graph*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg](https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg)

```
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg](https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg)

```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1

```

**Constraints:**

- `1 <= n <= 2000`
- `1 <= edges.length <= 5000`
- `edges[i].length == 2`
- `0 <= ai <= bi < n`
- `ai != bi`
- There are no repeated edges.

无向图联通块 Union find/BFS

```cpp
//union find
class Solution {
public:
    vector<int> father;
    
    int find(int x){
        if(father[x]==x) return x;
        father[x] = find(father[x]);
        return father[x];
    }
        
    bool connect(int a, int b){
        int fa=find(a);
        int fb=find(b);
        if(fa==fb) return false;
        father[fb]=fa;
        return true;
    }
        
    void init(int n){
        father.resize(n+1,0);
        for(int i=0; i<n+1; i++){
            father[i]=i;
        }
    }
    
    int countComponents(int n, vector<vector<int>>& edges) {
        int res=n;
        init(n);
        for(auto e:edges){
            int a=e[0], b=e[1];
            if(connect(a,b)){res--;}
        }
        return res;
    }
};
```

```cpp
//bfs
class Solution {
public:
    int countComponents(int n, vector<vector<int>>& edges) {
        //build graph
        if(n==0) return 0;
        vector<unordered_set<int>> g(n,unordered_set<int>());
        for(auto e: edges){
            g[e[0]].insert(e[1]);
            g[e[1]].insert(e[0]);
        }
        //bfs
        int cnt=0;
        vector<int> visited(n,0);
        for(int i=0; i<n; i++){
            if(!visited[i]) {
                cnt++;
                bfs(g,visited,i);
            }
        }
        return cnt;
    }
    
    void bfs(vector<unordered_set<int>>& g, vector<int>& visited, int root){
        queue<int> q;
        visited[root]=true;
        q.push(root);
        while(!q.empty()){
            int cur=q.front(); q.pop();
            for(auto nei: g[cur]){
                if(visited[nei]) continue;
                visited[nei]=true;
                q.push(nei);
            }
        }
    }
};
```