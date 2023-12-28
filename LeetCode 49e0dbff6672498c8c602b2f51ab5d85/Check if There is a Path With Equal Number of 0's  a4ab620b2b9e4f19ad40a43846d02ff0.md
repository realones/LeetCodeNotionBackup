# Check if There is a Path With Equal Number of 0's And 1's

#: 2510
Difficult: Medium
Tags: DP
link: https://leetcode.com/problems/check-if-there-is-a-path-with-equal-number-of-0s-and-1s/
Priority: High
Status: Dig More, No Idea At First
Created time: June 15, 2023 5:40 PM
Last edited time: December 16, 2023 2:19 AM
source: leetcode_daily
related: Valid Parenthesis String (Valid%20Parenthesis%20String%200d92dc8e30424a248b4811fcfadbd984.md)

You are given a **0-indexed** `m x n` **binary** matrix `grid`. You can move from a cell `(row, col)` to any of the cells `(row + 1, col)` or `(row, col + 1)`.

Return `true` *if there is a path from* `(0, 0)` *to* `(m - 1, n - 1)` *that visits an **equal** number of* `0`*'s and* `1`*'s*. Otherwise return `false`.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/12/20/yetgriddrawio-4.png](https://assets.leetcode.com/uploads/2022/12/20/yetgriddrawio-4.png)

```
Input: grid = [[0,1,0,0],[0,1,0,0],[1,0,1,0]]
Output: true
Explanation: The path colored in blue in the above diagram is a valid path because we have 3 cells with a value of 1 and 3 with a value of 0. Since there is a valid path, we return true.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/12/20/yetgrid2drawio-1.png](https://assets.leetcode.com/uploads/2022/12/20/yetgrid2drawio-1.png)

```
Input: grid = [[1,1,0],[0,0,1],[1,0,0]]
Output: false
Explanation: There is no path in this grid with an equal number of 0's and 1's.

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 100`
- `grid[i][j]` is either `0` or `1`.

comment: need to prove that: for all path from (x,y) to (x2,y2), all possible num of 1s is in a discretely continuously range [min1s, max1s], e.i. all min, min+1, min+2…max, there is a path with that num of 1s

Solution from ans:

```cpp
int MIN=0, MAX=1;

class Solution {
public:
    bool isThereAPath(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        if((m+n-1)%2!=0) return false;
        int target=(m+n)/2;//shoudld be target 1s in the path from 00 to bb
        vector<vector<vector<int>>> dp(m,vector<vector<int>>(n,vector<int>(2,0)));
        //init
        dp[0][0][MIN]=dp[0][0][MAX]=matrix[0][0];
        for(int i=1; i<m; i++){
            dp[i][0][MIN]= dp[i-1][0][MIN] + matrix[i][0];
            dp[i][0][MAX]= dp[i-1][0][MAX] + matrix[i][0];
        }
        for(int j=1; j<n; j++){
            dp[0][j][MIN]= dp[0][j-1][MIN] + matrix[0][j];
            dp[0][j][MAX]= dp[0][j-1][MAX] + matrix[0][j];
        }
        //process
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j][MIN]= min(dp[i-1][j][MIN], dp[i][j-1][MIN]) + matrix[i][j];
                dp[i][j][MAX]= max(dp[i-1][j][MAX], dp[i][j-1][MAX]) + matrix[i][j];
            }
        }
        return dp.back().back()[MIN]<=target && target<=dp.back().back()[MAX];
    }
};
```

Solution TLE: dp

```cpp
class Solution {
public:
    bool isThereAPath(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        int M=m*n;
        
        vector<vector<vector<bool>>> dp(m,vector<vector<bool>>(n,vector<bool>(m*n+1+M,false)));
        
        int k=0;
        for(int i=0; i<m; i++){
            k+=matrix[i][0]==0?-1:1;
            dp[i][0][k+M]=true;
        }
        k=0;
        for(int j=0; j<n; j++){
            k+=matrix[0][j]==0?-1:1;
            dp[0][j][k+M]=true;
        }
        
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                int cur=matrix[i][j]==0?-1:1;
                for(int k=-m*n;k<m*n+1;k++){
                    if(0<=k-cur+M && k-cur+M<=m*n+1+M){
                        dp[i][j][k+M]=dp[i-1][j][k-cur+M] || dp[i][j-1][k-cur+M];
                    }
                }
            }
        }
        
        return dp[m-1][n-1][0+M];
    }
};
```

Solution TLE: recur top-down + memo

```cpp
class Solution {
public:
    bool isThereAPath(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        vector<vector<unordered_map<int,int>>> dp(m,vector<unordered_map<int,int>>(n));
        //0:-1, 1:1
        function<bool(int,int,int)>  f=[&](int x, int y, int req)->bool{
            if(dp[x][y].count(req)) return dp[x][y][req];

            if(matrix[x][y]==0) req++;//need 1
            else req--;//need 0

            if(x==m-1 && y==n-1) return req==0;
            if(x<m-1){
                if(f(x+1,y,req)) {dp[x][y][req]=true; return true;}
            }
            if(y<n-1){
                if(f(x,y+1,req)) {dp[x][y][req]=true; return true;}
            }
            dp[x][y][req]=false; return false;
        };
        //call f
        return f(0,0,0);
    }
};
```