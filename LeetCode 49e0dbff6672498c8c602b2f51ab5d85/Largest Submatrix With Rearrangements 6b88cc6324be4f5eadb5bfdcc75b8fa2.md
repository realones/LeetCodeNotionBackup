# Largest Submatrix With Rearrangements

#: 1727
Difficult: Medium
Tags: DP, Greedy, Matrix, Prefix, Sort
link: https://leetcode.com/problems/largest-submatrix-with-rearrangements/
Priority: High
Status: No Idea At First
Created time: June 27, 2023 10:05 PM
Last edited time: November 27, 2023 12:40 AM
source: HuaHua, leetcode_daily

You are given a binary matrix `matrix` of size `m x n`, and you are allowed to rearrange the **columns** of the `matrix` in any order.

Return *the area of the largest submatrix within* `matrix` *where **every** element of the submatrix is* `1` *after reordering the columns optimally.*

**Example 1:**

![https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40536-pm.png](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40536-pm.png)

```
Input: matrix = [[0,0,1],[1,1,1],[1,0,1]]
Output: 4
Explanation: You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 4.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40852-pm.png](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40852-pm.png)

```
Input: matrix = [[1,0,1,0,1]]
Output: 3
Explanation: You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 3.

```

**Example 3:**

```
Input: matrix = [[1,1,0],[1,0,1]]
Output: 2
Explanation: Notice that you must rearrange entire columns, and there is no way to make a submatrix of 1s larger than an area of 2.

```

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m * n <= 105`
- `matrix[i][j]` is either `0` or `1`.

Solution:

reorder → sort

matrix → calcu for each row, with prefix

```cpp
class Solution {
public:
    int largestSubmatrix(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        for(int i=1;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==0) continue;
                else matrix[i][j]=matrix[i-1][j]+1;
            }
        }

        int res=0;
        for(int i=0;i<m;i++){
            sort(matrix[i].begin(), matrix[i].end(), greater<>());
            int len=0;
            for(int j=0;j<n;j++){
                if(matrix[i][j]==0) continue;
                len++;
                int tmp= matrix[i][j]*len;
                res=max(res,tmp);
            }
        }
        return res;
    }
};

/*
m*n: 1~1e5
val: 0 or 1
================================
op: rearrage columns
return: largest area of submatrix with all elems == 1
================================
for high: O(1e5)
    for low: O(1e5)
        check how many colomn satisfies: O(1e5)
TLE
================================
from hint:
for each col: find num of consecutive ones ending at each position:
-> matrix[i][j]= cnt of consecutive 1s till and w/ cur
overall: O(mn)

for each row:
    sort by matrix[i][j] reversely and find max rectangle with bottom edge on cur row
overall: O(mnlogmn + mn)

subqus: how to find max rectangle from reversed sorted arr
    res max= curHigh * len
*/
```