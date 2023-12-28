# Rotting Oranges

#: 994
Difficult: Medium
Tags: BFS, Graph, Matrix
link: https://leetcode.com/problems/rotting-oranges
Priority: Low
Created time: July 10, 2023 3:01 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

You are given an `m x n` `grid` where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return *the minimum number of minutes that must elapse until no cell has a fresh orange*. If *this is impossible, return* `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/02/16/oranges.png](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

```
Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4

```

**Example 2:**

```
Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

```

**Example 3:**

```
Input: grid = [[0,2]]
Output: 0
Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` is `0`, `1`, or `2`.

```cpp
using ii=pair<int,int>;

class Solution {
public:
    vector<int> d{-1,0,1,0,-1};
    
    int orangesRotting(vector<vector<int>>& matrix) {
        int m=matrix.size(); 
        int n=matrix[0].size();
        queue<ii> q;

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==2) q.emplace(i,j);
            }
        }

        int step=0;
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                auto [x,y] = q.front(); q.pop();
                for(int i=0; i<4; i++){
                    int nx = x + d[i];
                    int ny = y + d[i+1];
                    if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                    if(matrix[nx][ny]==1) {
                        matrix[nx][ny]=2;
                        q.emplace(nx,ny);
                    }
                }
            }
            step++;
        }

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1) return -1;
            }
        }
        
        return max(0,step-1);
    }
};
```