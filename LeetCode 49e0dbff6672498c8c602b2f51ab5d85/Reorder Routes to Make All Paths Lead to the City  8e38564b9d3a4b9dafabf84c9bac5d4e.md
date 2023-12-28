# Reorder Routes to Make All Paths Lead to the City Zero

#: 1466
Difficult: Medium
Tags: BFS, DFS, Graph
link: https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero
Priority: Low
Created time: July 10, 2023 2:53 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

There are `n` cities numbered from `0` to `n - 1` and `n - 1` roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by `connections` where `connections[i] = [ai, bi]` represents a road from city `ai` to city `bi`.

This year, there will be a big event in the capital (city `0`), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city `0`. Return the **minimum** number of edges changed.

It's **guaranteed** that each city can reach city `0` after reorder.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/05/13/sample_1_1819.png](https://assets.leetcode.com/uploads/2020/05/13/sample_1_1819.png)

```
Input: n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
Output: 3
Explanation:Change the direction of edges show in red such that each node can reach the node 0 (capital).

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/05/13/sample_2_1819.png](https://assets.leetcode.com/uploads/2020/05/13/sample_2_1819.png)

```
Input: n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
Output: 2
Explanation:Change the direction of edges show in red such that each node can reach the node 0 (capital).

```

**Example 3:**

```
Input: n = 3, connections = [[1,0],[2,0]]
Output: 0

```

**Constraints:**

- `2 <= n <= 5 * 104`
- `connections.length == n - 1`
- `connections[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`

```cpp
class Solution {
public:  
    int minReorder(int n, vector<vector<int>>& connections) {
        unordered_map<int, unordered_set<int>> mp2;//graph - 2d
        unordered_map<int, unordered_set<int>> mp;//graph - 1d
        vector<int> visited(n,0);

        for(auto& c: connections){
            int a=c[0], b=c[1];
            mp[a].insert(b);
            mp2[a].insert(b);
            mp2[b].insert(a);
        }

        int res=0;
        queue<int> q;
        visited[0]=1;
        q.push(0);
        while(!q.empty()){
            int cur=q.front(); q.pop();
            for(auto nei: mp2[cur]){
                if(visited[nei]) continue;
                visited[nei]=true;
                q.push(nei);
                if(mp[cur].count(nei)) res++;
            }
        }
        return res;
    }
};

/*
n: 2~5e4
conn: 
    len: n-1
    val: valid
    tree-> no circle, each one can reach

start from 0, bfs/dfs to next one, if edge is reversed, reversed the edge
*/
```