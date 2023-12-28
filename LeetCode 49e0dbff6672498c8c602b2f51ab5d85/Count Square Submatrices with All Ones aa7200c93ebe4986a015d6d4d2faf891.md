# Count Square Submatrices with All Ones

#: 1277
Difficult: Medium
Tags: DP, Matrix
link: https://leetcode.com/problems/count-square-submatrices-with-all-ones/
Priority: Medium
Created time: September 23, 2023 10:33 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Maximal Square (Maximal%20Square%20b0ab6ceef27642839f18680f1f872a0f.md)

Given a `m * n` matrix of ones and zeros, return how many **square** submatrices have all ones.

**Example 1:**

```
Input: matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
Output: 15
Explanation:
There are10 squares of side 1.
There are4 squares of side 2.
There is1 square of side 3.
Total number of squares = 10 + 4 + 1 =15.

```

**Example 2:**

```
Input: matrix =
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
Output: 7
Explanation:
There are6 squares of side 1.
There is1 square of side 2.
Total number of squares = 6 + 1 =7.

```

**Constraints:**

- `1 <= arr.length <= 300`
- `1 <= arr[0].length <= 300`
- `0 <= arr[i][j] <= 1`

```cpp
class Solution {
public:
    int countSquares(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0, m,n: 1~300, val:0or1
        vector<vector<int>> dp(m+1,vector<int>(n+1,0));
        int res=0;
        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(matrix[i-1][j-1]==0) continue;
                dp[i][j]=min({dp[i-1][j-1], dp[i][j-1], dp[i-1][j]}) +1;
                res+=dp[i][j];
            }
        }
        return res;
    }
};

/*
for each cell
    calcu the max len of square with rightBottom at cur cell
    res+=len
*/
```