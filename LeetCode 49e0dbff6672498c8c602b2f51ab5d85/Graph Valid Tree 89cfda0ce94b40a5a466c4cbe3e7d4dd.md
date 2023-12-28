# Graph Valid Tree

#: 261
Difficult: Medium
Tags: BFS, DFS, Graph, Union Find, gbfs_postChkVisited, gbfs_preChkVisited_woSz
link: https://leetcode.com/problems/graph-valid-tree/
Priority: Medium
Created time: July 17, 2022 11:52 PM
Last edited time: October 16, 2023 1:00 AM
practicedTimes: 1
source: jiuzhang, 算初BFS, 算高UnionFind&Trie

You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer n and a list of `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the graph.

Return `true` *if the edges of the given graph make up a valid tree, and* `false` *otherwise*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/12/tree1-graph.jpg](https://assets.leetcode.com/uploads/2021/03/12/tree1-graph.jpg)

```
Input: n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
Output: true

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/03/12/tree2-graph.jpg](https://assets.leetcode.com/uploads/2021/03/12/tree2-graph.jpg)

```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]
Output: false

```

**Constraints:**

- `1 <= n <= 2000`
- `0 <= edges.length <= 5000`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- There are no self-loops or repeated edges.

**图**：If BFS can solve, never use DFS! - overflow issue

Hashmap（去重复）

1.no cicle - check visited

2. no isolation

1. Union Find: n==edges.size()+1
2. BFS: visited.size()==n

solution 1: BFS
solution 2: union find - use existing graph, not need to compress path - O(n)

- Make sure there is no cycle in the graph - this has to be a none-cyclic graph;
    - union find - union operation should not fail (two node should not have same father)
- Make sure every node is reached && this has to be one connected graph;
    - edge num: n-1

```cpp
//union find
class Solution {
public:
    bool validTree(int n, vector<pair<int, int>>& edges) {
        //check edge size
        if(edges.size()!=n-1) return false;
        
        //union find
				//init
        vector<int> father(n);
        for(int i=0;i<n;i++){father[i]=i;}
        
				//union
        for(auto e:edges){
            int f=e.first;
            int s=e.second;
            //find root
            while(f!=father[f]){f=father[f];}
            while(s!=father[s]){s=father[s];}
            //check
            if(f==s) return false;
            
            //union
            father[s]=f;
        }
        return true;
    }
};
```

```jsx
//bfs
class Solution {
public:
    bool validTree(int n, vector<vector<int>>& edges) {
        if(n-1 != edges.size()) return false;
        vector<bool> visited(n,0);
        unordered_map<int,unordered_set<int>> mp;
        for(auto& e: edges){
            mp[e[0]].insert(e[1]);
            mp[e[1]].insert(e[0]);
        }
        queue<int> q;
        visited[0]=1;
        q.push(0);
        while(!q.empty()){
            int cur=q.front(); q.pop();
            
            for(int nei: mp[cur]){
                if(visited[nei]) return false;
                visited[nei]=1;
                mp[nei].erase(cur);
                q.push(nei);
            }
        }
        
        for(auto v: visited){
            if(!v) return false;
        }
        return true;
    }
};
/*
n: 0..n-1
edge: p1,p2
*/
```

```cpp
//deprecated to above solution
class Solution {
public:
    /**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    bool validTree(int n, vector<vector<int>> &edges) {
        // write your code here
        if(n==0 && edges.empty()) return true;
        //if(n==0 || edges.empty()) return false; //incorrect, 1,[] is true
        //build map, can't put smaller as head, as 0 might not be the root
        unordered_map<int,unordered_set<int>> mp;//can update to vector instead of map
        for(auto e:edges){
            mp[e[0]].insert(e[1]);
            mp[e[1]].insert(e[0]);
        }
        //check if circle
        //dfs
        unordered_set<int> visited;
        queue<int> q;
        q.push(0);//start from any
       
        while(!q.empty()){
            int cur=q.front();q.pop();
            if(visited.count(cur)) return false;
            visited.insert(cur);
            unordered_set<int> children=mp[cur];//!!!never use directly in for loop
            for(auto child: children){
                q.push(child);
                //!!! update graph
                mp[cur].erase(child);//not needed
                mp[child].erase(cur);
            }
        }
        
        //check isolatoin
        return visited.size()==n;
    }
};
```