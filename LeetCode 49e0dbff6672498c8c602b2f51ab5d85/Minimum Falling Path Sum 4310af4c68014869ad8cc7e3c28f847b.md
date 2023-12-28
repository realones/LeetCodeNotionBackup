# Minimum Falling Path Sum

#: 931
Difficult: Medium
Tags: DP, Matrix
link: https://leetcode.com/problems/minimum-falling-path-sum/
Priority: Low
Created time: September 23, 2023 1:08 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an `n x n` array of integers `matrix`, return *the **minimum sum** of any **falling path** through* `matrix`.

A **falling path** starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position `(row, col)` will be `(row + 1, col - 1)`, `(row + 1, col)`, or `(row + 1, col + 1)`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/11/03/failing1-grid.jpg](https://assets.leetcode.com/uploads/2021/11/03/failing1-grid.jpg)

```
Input: matrix = [[2,1,3],[6,5,4],[7,8,9]]
Output: 13
Explanation: There are two falling paths with a minimum sum as shown.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/11/03/failing2-grid.jpg](https://assets.leetcode.com/uploads/2021/11/03/failing2-grid.jpg)

```
Input: matrix = [[-19,57],[-40,-5]]
Output: -59
Explanation: The falling path with a minimum sum is shown.

```

**Constraints:**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 100`
- `100 <= matrix[i][j] <= 100`

Solution matrix dp:

```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        vector<vector<int>> dp(m+1,vector<int>(n+2,INT_MAX));
        for(int j=1; j<n+1; j++) dp[0][j]=0;
        for(int i=1;i<m+1;i++){//ij for dp, xy for matrix
            for(int j=1;j<n+1;j++){
                int x=i-1, y=j-1;
                dp[i][j]= matrix[x][y] + min({
                                                dp[i-1][j-1],
                                                dp[i-1][j],
                                                dp[i-1][j+1]
                                            });
            }
        }
        return *min_element(dp.back().begin(), dp.back().end());
    }
};

/*
add shell:
XXXXX
X   X
X   X
*/
```