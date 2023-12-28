# As Far from Land as Possible

#: 1162
Difficult: Medium
Tags: BFS, DP, Graph, Math
link: https://leetcode.com/problems/as-far-from-land-as-possible/description/
Priority: Low
Created time: August 24, 2023 12:47 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an `n x n` `grid` containing only values `0` and `1`, where `0` represents water and `1` represents land, find a water cell such that its distance to the nearest land cell is maximized, and return the distance. If no land or water exists in the grid, return `-1`.

The distance used in this problem is the Manhattan distance: the distance between two cells `(x0, y0)` and `(x1, y1)` is `|x0 - x1| + |y0 - y1|`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/05/03/1336_ex1.JPG](https://assets.leetcode.com/uploads/2019/05/03/1336_ex1.JPG)

```
Input: grid = [[1,0,1],[0,0,0],[1,0,1]]
Output: 2
Explanation: The cell (1, 1) is as far as possible from all the land with distance 2.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/05/03/1336_ex2.JPG](https://assets.leetcode.com/uploads/2019/05/03/1336_ex2.JPG)

```
Input: grid = [[1,0,0],[0,0,0],[0,0,0]]
Output: 4
Explanation: The cell (2, 2) is as far as possible from all the land with distance 4.

```

**Constraints:**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 100`
- `grid[i][j]` is `0` or `1`

comment: where there is a dp tag?

```cpp
class Solution {
public:
    vector<int> d{-1,0,1,0,-1};
    using ii=pair<int,int>;

    int maxDistance(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        
        queue<ii> q;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1) {
                    q.emplace(i,j);
                }
            }
        }

        int res=0;
        while(!q.empty()){
            auto [x,y] = q.front(); q.pop();
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]==0) {
                    matrix[nx][ny]=matrix[x][y]+1;
                    res=max(res, matrix[nx][ny]);
                    q.emplace(nx,ny);
                }
            }
        }
        return res-1;
    }
};
```