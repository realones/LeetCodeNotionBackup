# Longest Increasing Path in a Matrix

#: 329
Difficult: Hard
Tags: DFS, DP, Graph, Matrix, search
link: https://leetcode.com/problems/longest-increasing-path-in-a-matrix/
Priority: Low
Status: More Solution
Created time: October 6, 2022 8:45 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, googleFreq

Given an `m x n` integers `matrix`, return *the length of the longest increasing path in* `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg](https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg)

```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is[1, 2, 6, 9].

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg](https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg)

```
Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
Output: 4
Explanation:The longest increasing path is[3, 4, 5, 6]. Moving diagonally is not allowed.

```

**Example 3:**

```
Input: matrix = [[1]]
Output: 1

```

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `0 <= matrix[i][j] <= 231 - 1`

Solution; 这题凭啥是hard？

```cpp
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        vector<vector<int>> f(m,vector<int>(n,0));
        
        int res=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                res=max(res,dfs(matrix, f, i, j));
            }
        }
        return res;
    }
    
    vector<int> d{0,-1,0,1,0};
    int dfs(vector<vector<int>>& matrix, vector<vector<int>>& f, int x, int y){
        int m=matrix.size();
        int n=matrix[0].size();
        
        if(f[x][y]) return f[x][y];
        int res=1;
        
        for(int k=0;k<4;k++){
            int nx=x+d[k];
            int ny=y+d[k+1];
            if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
            if(matrix[nx][ny]>=matrix[x][y]) continue;
            res=max(res,dfs(matrix,f,nx,ny)+1);
        }
        
        f[x][y]=res;
        return res;
    }
};
```