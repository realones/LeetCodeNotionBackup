# Unique Paths II

#: 63
Difficult: Medium
Tags: DP, Matrix
link: https://leetcode.com/problems/unique-paths-ii/
Priority: Pass
Created time: June 5, 2023 12:04 AM
Last edited time: November 2, 2023 5:51 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150

You are given an `m x n` integer array `grid`. There is a robot initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

An obstacle and space are marked as `1` or `0` respectively in `grid`. A path that the robot takes cannot include **any** square that is an obstacle.

Return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.

The testcases are generated so that the answer will be less than or equal to `2 * 109`.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

```
Input: obstacleGrid = [[0,1],[0,0]]
Output: 1

```

**Constraints:**

- `m == obstacleGrid.length`
- `n == obstacleGrid[i].length`
- `1 <= m, n <= 100`
- `obstacleGrid[i][j]` is `0` or `1`.

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        if(matrix[0][0]==1) return 0;
        vector<vector<int>> dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i==0 && j==0) {dp[i][j]=1;continue;}
                if(matrix[i][j]==0){
                    dp[i][j]=
                        (i-1>=0?dp[i-1][j]:0) + 
                        (j-1>=0?dp[i][j-1]:0);
                }
                else{
                    dp[i][j]=0;
                }
            }
        }
        
        return dp.back().back();
    }
};
```