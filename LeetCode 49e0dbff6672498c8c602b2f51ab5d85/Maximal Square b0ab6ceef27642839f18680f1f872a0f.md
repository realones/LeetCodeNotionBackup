# Maximal Square

#: 221
Difficult: Medium
Tags: DP, Matrix
link: https://leetcode.com/problems/maximal-square/
Priority: Low
Created time: September 9, 2022 1:03 PM
Last edited time: October 29, 2023 10:49 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算高DP上
related: Count Square Submatrices with All Ones (Count%20Square%20Submatrices%20with%20All%20Ones%20aa7200c93ebe4986a015d6d4d2faf891.md)

Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, *find the largest square containing only* `1`'s *and return its area*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

```
Input: matrix = [["0","1"],["1","0"]]
Output: 1

```

**Example 3:**

```
Input: matrix = [["0"]]
Output: 0

```

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 300`
- `matrix[i][j]` is `'0'` or `'1'`.

**原理**：左上角的三个小正方加上当前的cell形成更大的正方形。

1. 状态 State

f[i][j] 表示以i和j作为正方形右下角可以拓展的最大边长

2. 方程 Function

if matrix[i][j] == 1

f[i][j] = min(LEFT[i][j-1], UP[i-1][j], f[i-1][j-1]) + 1;

f[i%2][j] = min(f[(i - 1)%2][j], f[i%2][j-1], f[(i-1)%2][j-1]) + 1;

if matrix[i][j] == 0

f[i][j] = 0

f[i%2][j] = 0

3. 初始化 Intialization

f[i][0] = matrix[i][0];  -> f[i%2][0] = matrix[i][0];

f[0][j] = matrix[0][j];  -> f[0][j] = matrix[0][j];

4. 答案 Answer

max{f[i][j]} -> max{f[i%2][j]}

```cpp
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if(matrix.size()==0 || matrix[0].size()==0) return 0;
        int m=matrix.size(),n=matrix[0].size();
        int len=0;
        vector<vector<int>> tmp(2,vector<int>(n,0));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]=='0'){tmp[i%2][j]=0;continue;}
                else{
                    if(i==0||j==0){tmp[i%2][j]=1;}
                    else{tmp[i%2][j]=min(min(tmp[(i-1)%2][(j-1)],tmp[(i-1)%2][j]),tmp[i%2][(j-1)])+1;}
                }
                len=max(len,tmp[i%2][j]);
            }
        }
        return len*len;
    }
};
```