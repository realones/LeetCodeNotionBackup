# Shortest Bridge

#: 934
Difficult: Medium
Tags: BFS, Matrix
link: https://leetcode.com/problems/shortest-bridge/
Priority: Low
Created time: May 23, 2023 4:25 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

You are given an `n x n` binary matrix `grid` where `1` represents land and `0` represents water.

An **island** is a 4-directionally connected group of `1`'s not connected to any other `1`'s. There are **exactly two islands** in `grid`.

You may change `0`'s to `1`'s to connect the two islands to form **one island**.

Return *the smallest number of* `0`*'s you must flip to connect the two islands*.

**Example 1:**

```
Input: grid = [[0,1],[1,0]]
Output: 1

```

**Example 2:**

```
Input: grid = [[0,1,0],[0,0,0],[0,0,1]]
Output: 2

```

**Example 3:**

```
Input: grid = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
Output: 1

```

**Constraints:**

- `n == grid.length == grid[i].length`
- `2 <= n <= 100`
- `grid[i][j]` is either `0` or `1`.
- There are exactly two islands in `grid`.

```cpp
class Solution {
public:
    vector<int> d{-1,0,1,0,-1};
    typedef pair<int, int> Block;
    
    int shortestBridge(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        //find x,y for 1st island
        int x=0,y=0;
        bool found=false;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1){ x=i,y=j; found=true; break; }
            }
            if(found) break;
        }
        
        //put 1st island coordinate to q
        bfs(matrix,{x,y});
        
        //find 2nd island
        queue<Block> q;
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==2) {
                    q.push({i,j});
                }
            }
        }
        
        int res=0;
        while(!q.empty()){
            int qSz=q.size();
            for(int i=0; i<qSz; i++){
                Block cur = q.front(); q.pop();
                int x=cur.first, y=cur.second;
                for(int i=0; i<4; i++){
                    int nx = x + d[i];
                    int ny = y + d[i+1];
                    if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                    if(matrix[nx][ny]==0) {
                        matrix[nx][ny]=2;
                        q.push({nx,ny});
                    }
                    if(matrix[nx][ny]==1) {return res;}
                }
            }
            res++;
        }
        return -1;
    }

    void bfs(vector<vector<int>>& matrix, Block root){
        int m=matrix.size(); 
        int n=matrix[0].size();
        queue<Block> q;

        matrix[root.first][root.second] = 2;//mark as visited
        q.push(root);

        while(!q.empty()){
            Block cur = q.front(); q.pop();
            int x=cur.first, y=cur.second;
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]==1) {
                    matrix[nx][ny]=2;
                    q.push({nx,ny});
                }
            }
        }
    }
};
```