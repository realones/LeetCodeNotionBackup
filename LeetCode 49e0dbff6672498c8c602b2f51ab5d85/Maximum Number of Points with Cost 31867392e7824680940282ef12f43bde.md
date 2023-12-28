# Maximum Number of Points with Cost

#: 1937
Difficult: Medium
Tags: DP, Matrix
link: https://leetcode.com/problems/maximum-number-of-points-with-cost/
Priority: High
Status: No Idea At First
Created time: October 24, 2022 2:26 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

You are given an `m x n` integer matrix `points` (**0-indexed**). Starting with `0` points, you want to **maximize** the number of points you can get from the matrix.

To gain points, you must pick one cell in **each row**. Picking the cell at coordinates `(r, c)` will **add** `points[r][c]` to your score.

However, you will lose points if you pick a cell too far from the cell that you picked in the previous row. For every two adjacent rows `r` and `r + 1` (where `0 <= r < m - 1`), picking cells at coordinates `(r, c1)` and `(r + 1, c2)` will **subtract** `abs(c1 - c2)` from your score.

Return *the **maximum** number of points you can achieve*.

`abs(x)` is defined as:

- `x` for `x >= 0`.
- `x` for `x < 0`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-40-26-diagram-drawio-diagrams-net.png](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-40-26-diagram-drawio-diagrams-net.png)

```
Input: points = [[1,2,3],[1,5,1],[3,1,1]]
Output: 9
Explanation:
The blue cells denote the optimal cells to pick, which have coordinates (0, 2), (1, 1), and (2, 0).
You add 3 + 5 + 3 = 11 to your score.
However, you must subtract abs(2 - 1) + abs(1 - 0) = 2 from your score.
Your final score is 11 - 2 = 9.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-42-14-diagram-drawio-diagrams-net.png](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-42-14-diagram-drawio-diagrams-net.png)

```
Input: points = [[1,5],[2,3],[4,2]]
Output: 11
Explanation:
The blue cells denote the optimal cells to pick, which have coordinates (0, 1), (1, 1), and (2, 0).
You add 5 + 3 + 4 = 12 to your score.
However, you must subtract abs(1 - 1) + abs(1 - 0) = 1 from your score.
Your final score is 12 - 1 = 11.

```

**Constraints:**

- `m == points.length`
- `n == points[r].length`
- `1 <= m, n <= 105`
- `1 <= m * n <= 105`
- `0 <= points[r][c] <= 105`

Solution: left right DP array

[https://leetcode.com/problems/maximum-number-of-points-with-cost/discuss/1344908/C%2B%2BJavaPython-3-DP-Explanation-with-pictures-O(MN)-Time-O(N)-Space](https://leetcode.com/problems/maximum-number-of-points-with-cost/discuss/1344908/C%2B%2BJavaPython-3-DP-Explanation-with-pictures-O(MN)-Time-O(N)-Space)

```cpp
class Solution {
public:
    long long maxPoints(vector<vector<int>>& points) {
        int m=points.size(); if(m==0) {return 0;}
        int n=points[0].size(); if(n==0) {return 0;}
        
        vector<vector<long long>> dp(2,vector<long long>(n,0)); //can be improved by rolloing array
        dp[0]=vector<long long>(points[0].begin(), points[0].end());
        
        for(int i=1;i<m;i++){
            //get lft and rgt
            vector<long long> lft(n,0), rgt(n,0);
            vector<long long>& pre=dp[(i-1)%2];
            //lft
            for(int j=0;j<n;j++){
                lft[j]=max(pre[j], j-1>=0 ? lft[j-1]-1 : 0);
            }
            //rgt
            for(int j=n-1;j>=0;j--){
                rgt[j]=max(pre[j], j+1<=n-1 ? rgt[j+1]-1 : 0);
            }
                
            for(int j=0;j<n;j++){
                dp[i%2][j]=max(lft[j],rgt[j])+points[i][j];
            }
        }
        return *max_element(dp[(m-1)%2].begin(), dp[(m-1)%2].end());
    }
};

/*
T: m*n*n -> TLE
dp[i][j] = max(dp[i - 1][k] + points[i][j] - abs(j - k)) for each 0 <= k < points[i - 1].size()

T: m*n - no idea at first
left right dp
for dp pre row:
    for i 0...n-1
        lft[i]=max(pre[i],lft[i-1]-1);
    for i n-1...0
        rgt[i]=max(pre[i],rgt[i+1]-1);

for dp cur row:
    cur[i]=max(lft[i],rgt[i])+points[i,j]
*/
```