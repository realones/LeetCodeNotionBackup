# Minimum Falling Path Sum II

#: 1289
Difficult: Hard
Tags: DP, Matrix, dp-TimeSeqFromLastOne-Power
link: https://leetcode.com/problems/minimum-falling-path-sum-ii/
Priority: Low
Created time: November 19, 2022 10:53 PM
Last edited time: October 12, 2023 2:56 PM

Given an `n x n` integer matrix `grid`, return *the minimum sum of a **falling path with non-zero shifts***.

A **falling path with non-zero shifts** is a choice of exactly one element from each row of `grid` such that no two elements chosen in adjacent rows are in the same column.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/08/10/falling-grid.jpg](https://assets.leetcode.com/uploads/2021/08/10/falling-grid.jpg)

```
Input: arr = [[1,2,3],[4,5,6],[7,8,9]]
Output: 13
Explanation:
The possible falling paths are:
[1,5,9], [1,5,7], [1,6,7], [1,6,8],
[2,4,8], [2,4,9], [2,6,7], [2,6,8],
[3,4,8], [3,4,9], [3,5,7], [3,5,9]
The falling path with the smallest sum is [1,5,7], so the answer is 13.

```

**Example 2:**

```
Input: grid = [[7]]
Output: 7

```

**Constraints:**

- `n == grid.length == grid[i].length`
- `1 <= n <= 200`
- `99 <= grid[i][j] <= 99`

```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        
        int tmp=INT_MAX;
        for(int i=1;i<m;i++){
            for(int j=0;j<n;j++){
                swap(tmp, matrix[i-1][j]);
                matrix[i][j]+=*min_element(matrix[i-1].begin(), matrix[i-1].end());
                swap(tmp, matrix[i-1][j]);
            }
        }
        
        return *min_element(matrix.back().begin(), matrix.back().end());
    }
};
```