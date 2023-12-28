# Unique Paths

#: 62
Difficult: Medium
Tags: DP, GoForth, Matrix
link: https://leetcode.com/problems/unique-paths
Priority: Pass
Created time: August 17, 2022 11:25 PM
Last edited time: October 13, 2023 1:02 PM
practicedTimes: 1
source: HuaHua, LC_75, jiuzhang, 算初DP, 算高DP上

There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.

The test cases are generated so that the answer will be less than or equal to `2 * 109`.

**Example 1:**

![https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
Input: m = 3, n = 7
Output: 28

```

**Example 2:**

```
Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down

```

**Constraints:**

- `1 <= m, n <= 100`

Solution 1 - math combination:

```cpp
//机器人从左上角到右下角的移动可以被视为一个由 m-1 次向下移动和 n-1 次向右移动组成的序列。那么，问题就变成了：在 m+n-2 次移动中，选择 m-1 次向下移动（或选择 n-1 次向右移动）的不同方式有多少种。
class Solution {
public:
    int uniquePaths(int m, int n) {
        return combination(m+n-2, n-1);
    }

    long long combination(int n, int k) {
        if (k > n - k) k = n - k;  // C(n, k) = C(n, n-k)
        long long res = 1;
        for (int i = 0; i < k; ++i) {
            res = res * (n - i) / (i + 1);
        }
        return res;
    }
}
```

- state: f[x][y]从起点到x,y的路径数
- function: (研究倒数第一步) f[x][y] = f[x - 1][y] + f[x][y - 1]
- initialize:
    - f[0][i] = 1
    - f[i][0] = 1
- answer: f[n-1][m-1]
- Related Question: Unique Path II

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i==0 || j==0) {dp[i][j]=1;continue;}
                
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
            
        return dp[m-1][n-1];
    }
};
```