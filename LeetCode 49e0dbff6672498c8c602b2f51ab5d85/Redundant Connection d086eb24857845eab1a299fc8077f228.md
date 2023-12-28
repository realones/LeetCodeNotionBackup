# Redundant Connection

#: 684
Difficult: Medium
Tags: BFS, DFS, Graph, TopSorting, Tree, Union Find, circle
link: https://leetcode.com/problems/redundant-connection/description/
Priority: Medium
Status: Dig More, More Solution, toSummarize
Created time: June 27, 2023 11:52 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Find Eventual Safe States (Find%20Eventual%20Safe%20States%20293c6aca02624510871109460053e10e.md)

In this problem, a tree is an **undirected graph** that is connected and has no cycles.

You are given a graph that started as a tree with `n` nodes labeled from `1` to `n`, with one additional edge added. The added edge has two **different** vertices chosen from `1` to `n`, and was not an edge that already existed. The graph is represented as an array `edges` of length `n` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the graph.

Return *an edge that can be removed so that the resulting graph is a tree of* `n` *nodes*. If there are multiple answers, return the answer that occurs last in the input.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg)

```
Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg)

```
Input: edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
Output: [1,4]

```

**Constraints:**

- `n == edges.length`
- `3 <= n <= 1000`
- `edges[i].length == 2`
- `1 <= ai < bi <= edges.length`
- `ai != bi`
- There are no repeated edges.
- The given graph is connected.

comment: graph找circle里面的元素都有什么，我用了topsort，不过针对undirected graph有点怪，看看别解法，因为官方没写topSort tag

dig more: 其他解法怎么确定的last occurred edge

btw, DFS seems to be stupid alternative way of using unionFind, since it adding edge into graph one by one and do dfs each time to see if introduce circle

Solution - topSort

```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        //find circle
        int n=edges.size();
        //build graph
        unordered_map<int,unordered_set<int>> g;//to use vector
        unordered_map<int,int> ind;//to use vector
        for(auto& e: edges){
            int a=e[0], b=e[1];
            g[a].insert(b);
            g[b].insert(a);
            ind[a]++;
            ind[b]++;
        }
        //push nodes with 1 indegree into queue, since it's connected, not consider ==0
        queue<int> q;
        unordered_set<int> visited;
        for(int node=1;node<=n;node++){
            if(ind[node]<2){
                q.push(node);
                visited.insert(node);
            }
        }
        //top sort and process - bfs
        while(!q.empty()){
            int cur = q.front(); q.pop();
            //process
            //recalculate indegree of neighbors of cur
            for(int nei: g[cur]){
                if(visited.count(nei)) continue;
                ind[nei]--;
                if(ind[nei]<2){
                    visited.insert(nei);
                    q.push(nei);
                }
            }
        }
        //try remove edges
        for(int i=n-1;i>=0;i--){
            int a=edges[i][0], b=edges[i][1];
            if(!visited.count(a) && !visited.count(b)) return {a,b};
        }
        return {-1,-1};
    }
};

/*
node: 1,2,...,n
edges: [{a,b}]

return an edge than can be removed, return the last edge occurs in input

*/
```

Solution - Union Find

```cpp
class Solution {
public:
    //init
    vector<int> father;//use hash_map<type,type> if not using idx as identifier
    void init(int n) {//not required if using hash_map, init in begining of find()
        father.resize(n,0);
        for(int i=0;i<n;i++){
            father[i]=i;//parent is itself
        }
    }
    
    //with pass compression
    // O(1)
    int find(int a){
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }
    
    //union - union roots, tree like
    // O(1)
    bool Union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }

    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        //find circle
        int n=edges.size();
        init(n+1);

        for(auto& e: edges){
            int a=e[0], b=e[1];
            if(Union(a,b)) continue;
            else return {a,b};
        }
        return {-1,-1};
    }
};

/*
node: 1,2,...,n
edges: [{a,b}]

return an edge than can be removed, return the last edge occurs in input

*/
```

Solution BFS - Incorrect - not last occured edge

```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n=edges.size();
        //build graph
        vector<unordered_set<int>> g(n+1,unordered_set<int>());
        for(auto& e: edges){
            g[e[0]].insert(e[1]);
            g[e[1]].insert(e[0]);
        }
        
        //bfs
        vector<int> visited(n+1,0);
        queue<int> q;

        //since it is connected
        visited[1]=true;
        q.push(1);

        while(!q.empty()){
            int cur=q.front(); q.pop();
            //process
            unordered_set<int> tmp=g[cur];
            for(auto nei: tmp){
                if(visited[nei]) {
                    return {cur,nei};
                    continue;
                }
                visited[nei]=true;
                g[cur].erase(nei);
                g[nei].erase(cur);
                q.push(nei);
            }
        }
        return {-1,-1};
    }
};
```

Solution DFS

```cpp

```