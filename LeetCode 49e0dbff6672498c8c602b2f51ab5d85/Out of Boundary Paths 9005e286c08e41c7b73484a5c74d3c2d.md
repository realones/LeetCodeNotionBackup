# Out of Boundary Paths

#: 576
Difficult: Medium
Tags: DFS, DP, Matrix, recursion
link: https://leetcode.com/problems/out-of-boundary-paths/
Priority: Low
Created time: June 28, 2023 12:48 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

There is an `m x n` grid with a ball. The ball is initially at the position `[startRow, startColumn]`. You are allowed to move the ball to one of the four adjacent cells in the grid (possibly out of the grid crossing the grid boundary). You can apply **at most** `maxMove` moves to the ball.

Given the five integers `m`, `n`, `maxMove`, `startRow`, `startColumn`, return the number of paths to move the ball out of the grid boundary. Since the answer can be very large, return it **modulo** `109 + 7`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png)

```
Input: m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0
Output: 6

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_2.png](https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_2.png)

```
Input: m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1
Output: 12

```

**Constraints:**

- `1 <= m, n <= 50`
- `0 <= maxMove <= 50`
- `0 <= startRow < m`
- `0 <= startColumn < n`

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int findPaths(int m, int n, int maxMove, int startRow, int startColumn) {
        vector<vector<vector<ll>>> dp(m+2,vector<vector<ll>>(n+2,vector<ll>(maxMove+1,-1)));
        //init
        //m==0
        for(int i=0;i<m;i++)
            for(int j=0;j<n;j++)
                dp[i+1][j+1][0]=0;
        //out of edge
        for(int i=0; i<m; i++) 
            for(int k=0;k<=maxMove;k++)
                dp[i+1][0][k] = 1, dp[i+1].back()[k] = 1;
        for(int j=0; j<n; j++) 
            for(int k=0;k<=maxMove;k++)
                dp[0][j+1][k] = 1, dp.back()[j+1][k] = 1;
        //process
        function<ll(int,int,int)> f = [&](int i, int j, int k){//i,j for matrix, x,y for dp
            int x=i+1, y=j+1;
            if(dp[x][y][k]!=-1) return dp[x][y][k];
            dp[x][y][k] = 
                (f(i-1,j,k-1) + 
                f(i,j-1,k-1) + 
                f(i+1,j,k-1) + 
                f(i,j+1,k-1))%M ; 
            return dp[x][y][k];
        };
        return f(startRow,startColumn,maxMove);
    }
};

/*
dp[x][y][remainMove]= num of ways

dp[x][y][m] sum= dpU,L,R,D [m-1]

init:
dp[out of scope][any] = 1
dp[?][?][0]=0;

return: dp[startR][startC][maxMove]
*/
```