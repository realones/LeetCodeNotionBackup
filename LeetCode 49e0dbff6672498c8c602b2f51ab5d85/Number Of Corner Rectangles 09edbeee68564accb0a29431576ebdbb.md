# Number Of Corner Rectangles

#: 750
Difficult: Medium
Tags: Array, DP, Math, Matrix
link: https://leetcode.com/problems/number-of-corner-rectangles/description/
Priority: High
Status: More Solution
Created time: June 16, 2023 12:07 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an `m x n` integer matrix `grid` where each entry is only `0` or `1`, return *the number of **corner rectangles***.

A **corner rectangle** is four distinct `1`'s on the grid that forms an axis-aligned rectangle. Note that only the corners need to have the value `1`. Also, all four `1`'s used must be distinct.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/06/12/cornerrec1-grid.jpg](https://assets.leetcode.com/uploads/2021/06/12/cornerrec1-grid.jpg)

```
Input: grid = [[1,0,0,1,0],[0,0,1,0,1],[0,0,0,1,0],[1,0,1,0,1]]
Output: 1
Explanation: There is only one corner rectangle, with corners grid[1][2], grid[1][4], grid[3][2], grid[3][4].

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/06/12/cornerrec2-grid.jpg](https://assets.leetcode.com/uploads/2021/06/12/cornerrec2-grid.jpg)

```
Input: grid = [[1,1,1],[1,1,1],[1,1,1]]
Output: 9
Explanation: There are four 2x2 rectangles, four 2x3 and 3x2 rectangles, and one 3x3 rectangle.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/06/12/cornerrec3-grid.jpg](https://assets.leetcode.com/uploads/2021/06/12/cornerrec3-grid.jpg)

```
Input: grid = [[1,1,1,1]]
Output: 0
Explanation: Rectangles must have four distinct corners.

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `grid[i][j]` is either `0` or `1`.
- The number of `1`'s in the grid is in the range `[1, 6000]`.

```cpp
class Solution {
public:
    int countCornerRectangles(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        int res=0;
        vector<vector<int>> oneCnt(n,vector<int>(n,0));
        for(int i=0; i<m; i++){
            vector<int> v;//1 pos in cur row
            for(int j=0; j<n; j++){
                if(matrix[i][j]==1){
                    v.push_back(j);
                }
            }
            for(int l=0;l<v.size();l++){
                for(int r=l+1;r<v.size();r++){
                    res+=oneCnt[v[l]][v[r]];
                    oneCnt[v[l]][v[r]]++;
                }
            }
        }
        return res;
    }
};
```