# Count Negative Numbers in a Sorted Matrix

#: 1351
Difficult: Easy
Tags: BS_todo, Matrix
link: https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix/
Priority: Low
Created time: June 7, 2023 8:46 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given a `m x n` matrix `grid` which is sorted in non-increasing order both row-wise and column-wise, return *the number of **negative** numbers in* `grid`.

**Example 1:**

```
Input: grid = [[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]]
Output: 8
Explanation: There are 8 negatives number in the matrix.

```

**Example 2:**

```
Input: grid = [[3,2],[1,0]]
Output: 0

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `100 <= grid[i][j] <= 100`

**Follow up:**

Could you find an O(n + m) solution?

```cpp
class Solution {
public:
    int countNegatives(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        int res=0;
        int i=m-1, j=0;
        while(i>=0 && j<n){
            if(matrix[i][j]<0){
                res+=n-j;
                i--;
            }
            else{
                j++;
            }
        }
        return res;
    }
};
/*
[[4,3,2,-1]
,[3,2,1,-1]
,[1,1,-1,-2]
,[-1,-1,-2,-3]]

[[5,1,0]
,[-5,-5,-5]]
*/
```