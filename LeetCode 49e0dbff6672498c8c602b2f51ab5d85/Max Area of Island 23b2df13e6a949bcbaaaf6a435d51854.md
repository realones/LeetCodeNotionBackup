# Max Area of Island

#: 695
Difficult: Medium
Tags: BFS, Connected_Component, DFS, Graph, Matrix, Union Find
link: https://leetcode.com/problems/max-area-of-island/description/
Priority: Pass
Created time: August 23, 2023 11:51 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Making A Large Island (Making%20A%20Large%20Island%205be63eb9e13f4edeb73f83c249b6f51d.md)

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return *the maximum **area** of an island in* `grid`. If there is no island, return `0`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

```
Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.

```

**Example 2:**

```
Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`.

```cpp
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0

        int res=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1){
                    res=max(res, bfs(matrix,{i,j}));
                }
            }
        }
        return res;
    }

    //outside of function
    vector<int> d{-1,0,1,0,-1};
    using ii=pair<int,int>;

    int bfs(vector<vector<int>>& matrix, ii root){
        int m=matrix.size(); 
        int n=matrix[0].size();
        int res=0;
        queue<ii> q;

        matrix[root.first][root.second] = 2;//mark as visited
        q.push(root);
        res++;

        while(!q.empty()){
            auto [x,y] = q.front(); q.pop();
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]==1) {
                    matrix[nx][ny]=2;
                    res++;
                    q.emplace(nx,ny);
                }
            }
        }
        return res;
    }
};
```