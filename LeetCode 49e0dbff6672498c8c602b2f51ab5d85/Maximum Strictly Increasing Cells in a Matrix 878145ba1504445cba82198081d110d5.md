# Maximum Strictly Increasing Cells in a Matrix

#: 2713
Difficult: Hard
Tags: DP
link: https://leetcode.com/problems/maximum-strictly-increasing-cells-in-a-matrix/
Priority: High
Status: Dig More, noPatten
Created time: May 27, 2023 10:33 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

Given a **1-indexed** `m x n` integer matrix `mat`, you can select any cell in the matrix as your **starting cell**.

From the starting cell, you can move to any other cell **in the** **same row or column**, but only if the value of the destination cell is **strictly greater** than the value of the current cell. You can repeat this process as many times as possible, moving from cell to cell until you can no longer make any moves.

Your task is to find the **maximum number of cells** that you can visit in the matrix by starting from some cell.

Return *an integer denoting the maximum number of cells that can be visited.*

**Example 1:**

![https://assets.leetcode.com/uploads/2023/04/23/diag1drawio.png](https://assets.leetcode.com/uploads/2023/04/23/diag1drawio.png)

```
Input: mat = [[3,1],[3,4]]
Output: 2
Explanation: The image shows how we can visit 2 cells starting from row 1, column 2. It can be shown that we cannot visit more than 2 cells no matter where we start from, so the answer is 2.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/04/23/diag3drawio.png](https://assets.leetcode.com/uploads/2023/04/23/diag3drawio.png)

```
Input: mat = [[1,1],[1,1]]
Output: 1
Explanation: Since the cells must be strictly increasing, we can only visit one cell in this example.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2023/04/23/diag4drawio.png](https://assets.leetcode.com/uploads/2023/04/23/diag4drawio.png)

```
Input: mat = [[3,1,6],[-9,5,7]]
Output: 4
Explanation: The image above shows how we can visit 4 cells starting from row 2, column 1. It can be shown that we cannot visit more than 4 cells no matter where we start from, so the answer is 4.

```

**Constraints:**

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 105`
- `1 <= m * n <= 105`
- `105 <= mat[i][j] <= 105`

"longest path" problem.

Solution 1: no patten, not able to offer this answer

```cpp
class Solution {
public:
    int maxIncreasingCells(vector<vector<int>>& mat) {
        int m=mat.size(); if(m==0) {return 0;}
        int n=mat[0].size(); if(n==0) {return 0;}
        
        //sort position [i,j] by value in mat
        map<int, vector<int>> mp;//val - i*n+j
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                mp[mat[i][j]].push_back(i*n+j); } }
        
        vector<vector<int>> dp(m,vector<int>(n,0));
        vector<int> rMax(m,0);//max step for row[i]
        vector<int> cMax(n,0);
        //iterate cells in value increase order
        for(auto& [_,pos]: mp){//pos of i,j, from val small to large
            for(int p: pos){
                int i=p/n, j=p%n;
                dp[i][j] = max(rMax[i],cMax[j])+1;
            }
            for (int p : pos) {
                int i=p/n, j=p%n;
                cMax[j] = max(cMax[j], dp[i][j]);
                rMax[i] = max(rMax[i], dp[i][j]);
            }
        }
        
        return *max_element(rMax.begin(),rMax.end());
    }
};

/*
Time O(mnlogmn)
Space O(mn)
*/
```

Solution TLE

```cpp
class Solution {
public:
    int maxIncreasingCells(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        vector<vector<int>> dp(m,vector<int>(n,0));
        int res=1;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                res=max(res,helper(matrix,dp,i,j));
            }
        }
        return res;
    }
    
    int helper(vector<vector<int>>& matrix, vector<vector<int>>& dp, int x, int y){
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        if(dp[x][y]) return dp[x][y];
        
        int res=1;
        for(int i=0; i<m; i++){
            int nx=i, ny=y;
            if(nx==x) continue;
            if(matrix[x][y]>matrix[nx][ny]){
                res=max(res,helper(matrix,dp,nx,ny)+1);
            }
        }
        
        for(int j=0; j<n; j++){
            int nx=x, ny=j;
            if(ny==y) continue;
            if(matrix[x][y]>matrix[nx][ny]){
                res=max(res,helper(matrix,dp,nx,ny)+1);
            }
        }
        
        dp[x][y]=res;
        return res;
    }
};

//T: m*n * (m+n) -> TLE
```