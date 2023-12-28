# Find Eventual Safe States

#: 802
Difficult: Medium
Tags: BFS, DFS, Graph, TopSorting, circle
link: https://leetcode.com/problems/find-eventual-safe-states
Priority: High
Status: Dig More, More Solution, toRevisit
Created time: June 28, 2023 12:26 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily
related: Redundant Connection (Redundant%20Connection%20d086eb24857845eab1a299fc8077f228.md)

There is a directed graph of `n` nodes with each node labeled from `0` to `n - 1`. The graph is represented by a **0-indexed** 2D integer array `graph` where `graph[i]` is an integer array of nodes adjacent to node `i`, meaning there is an edge from node `i` to each node in `graph[i]`.

A node is a **terminal node** if there are no outgoing edges. A node is a **safe node** if every possible path starting from that node leads to a **terminal node** (or another safe node).

Return *an array containing all the **safe nodes** of the graph*. The answer should be sorted in **ascending** order.

**Example 1:**

![https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

```
Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
Output: [2,4,5,6]
Explanation: The given graph is shown above.
Nodes 5 and 6 are terminal nodes as there are no outgoing edges from either of them.
Every path starting at nodes 2, 4, 5, and 6 all lead to either node 5 or 6.
```

**Example 2:**

```
Input: graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
Output: [4]
Explanation:
Only node 4 is a terminal node, and every path starting at node 4 leads to node 4.

```

**Constraints:**

- `n == graph.length`
- `1 <= n <= 104`
- `0 <= graph[i].length <= n`
- `0 <= graph[i][j] <= n - 1`
- `graph[i]` is sorted in a strictly increasing order.
- The graph may contain self-loops.
- The number of edges in the graph will be in the range `[1, 4 * 104]`.

todo: 

花了37mins做出来，用了dfs+memo，很不熟练，一边写代码一边修改框架，dp和visited一开始搞混了，也没办法先写出清晰的dfs return和params，需要再练练，仔细想想

另外看到topsort tag，其实想想就是找环

Solution 1 DFS + Memo:

```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n=graph.size();//vertex: 1~1e4, edges: 1~4e4
        vector<int> res;
        vector<int> dp(n,-1);
        for(int i=0; i<n; i++){
            vector<int> visited(n,0);
            if(dfs(graph,i,dp,visited)) res.push_back(i);
        }
        return res;
    }

    //return: if safe/terminal-1 or infinite loop-0
    bool dfs(vector<vector<int>>& graph, int i, vector<int>& dp, vector<int>& visited){
        if(dp[i]!=-1) return dp[i];
        if(visited[i]==1) { return false; }
        bool res=true;
        visited[i]=1;
        for(auto nxt: graph[i]){
            if(!dfs(graph, nxt, dp,visited)) {
                res=false;
                break;
            }
        }
        dp[i]=res;
        return res;
    }
};

/*
every possible path starting from that node leads to a terminal node (or another safe node -- like recursive, safe one can be treated as terminal one)
either end to terminal one, or infinite loop, so check which one has infinite loop, and anyone goto infinite loop also has infinite loop
dfs+memo
T:O(n)
*/
```

solution: reverse seq + topSort

```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        vector<int> res;
        int n=graph.size();
        vector<unordered_set<int>> from_tos(n);
        unordered_map<int,int> ind;
        for(int to=0; to<n; to++){
            vector<int>& froms=graph[to];
            for(auto from: froms){
                from_tos[from].insert(to);
                ind[to]++;
            }
        }

        queue<int> q;
        for(int i=0; i<n; i++){
            if(ind[i]==0) q.push(i);
        }

        while(!q.empty()){
            int cur=q.front(); q.pop();
            res.push_back(cur);
            for(int nxt: from_tos[cur]){
                ind[nxt]--;
                if(ind[nxt]==0) q.push(nxt);
            }
        }

        sort(res.begin(), res.end());
        return res;
    }
};
```