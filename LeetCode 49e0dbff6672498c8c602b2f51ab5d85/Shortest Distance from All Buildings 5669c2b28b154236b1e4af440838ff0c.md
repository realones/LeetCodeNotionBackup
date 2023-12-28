# Shortest Distance from All Buildings

#: 317
Difficult: Hard
Tags: BFS, Matrix
link: https://leetcode.com/problems/shortest-distance-from-all-buildings/description/
Priority: Medium
Status: More Solution, toSummarize
Created time: December 4, 2023 9:53 PM
Last edited time: December 4, 2023 9:55 PM
source: facebookFreq

You are given an `m x n` grid `grid` of values `0`, `1`, or `2`, where:

- each `0` marks **an empty land** that you can pass by freely,
- each `1` marks **a building** that you cannot pass through, and
- each `2` marks **an obstacle** that you cannot pass through.

You want to build a house on an empty land that reaches all buildings in the **shortest total travel** distance. You can only move up, down, left, and right.

Return *the **shortest travel distance** for such a house*. If it is not possible to build such a house according to the above rules, return `-1`.

The **total travel distance** is the sum of the distances between the houses of the friends and the meeting point.

The distance is calculated using [Manhattan Distance](http://en.wikipedia.org/wiki/Taxicab_geometry), where `distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/03/14/buildings-grid.jpg](https://assets.leetcode.com/uploads/2021/03/14/buildings-grid.jpg)

```
Input: grid = [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]
Output: 7
Explanation: Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2).
The point (1,2) is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal.
So return 7.

```

**Example 2:**

```
Input: grid = [[1,0]]
Output: 1

```

**Example 3:**

```
Input: grid = [[1]]
Output: -1

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0`, `1`, or `2`.
- There will be **at least one** building in the `grid`.

comment:

m,n: 1~50
V: 250

so i assume it will TLE using BFS since T=O(v^2)

solutions for - to summarize solutions like this → multi to multi BFS

1. from 0 to 1
2. from 1 to 0

```cpp
//outside of function
vector<int> d{-1,0,1,0,-1};
using ii=pair<int,int>;

class Solution {
public:
    int shortestDistance(vector<vector<int>> matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        vector<vector<int>> total(m,vector<int>(n,0));

        int visitTime=0;
        //bfs
        auto bfs = [&](ii root)->void{
            queue<ii> q;
            matrix[root.first][root.second] = 2;//mark as visited
            q.push(root);
    
            int step=0;
            while(!q.empty()){
                int qSz=q.size();
                for(int i=0; i<qSz; i++){
                    auto [x,y] = q.front(); q.pop();
                    total[x][y]+=step;
                    for(int i=0; i<4; i++){
                        int nx = x + d[i];
                        int ny = y + d[i+1];
                        if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                        if(matrix[nx][ny]==visitTime) {
                            matrix[nx][ny]--;
                            q.emplace(nx,ny);
                        }
                    }
                }
                step++;
            }
        };

        //for each 1, bfs to 0s
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1){
                    bfs(ii{i,j});
                    visitTime--;
                }
            }
        }
        //res = min of total when mij==visitTime
        int res=INT_MAX;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==visitTime)
                    res=min(res, total[i][j]);
            }
        }
        return res==INT_MAX?-1:res;
    }
};
/*
0 emtpy
1 building, at least 1, also is a wall
2 wall

goal: find a 0, reach all 1s in shortest total dist
return: the shortest total dist, fallback: -1
================================
m,n: 1~50
V: 2500
================================
bfs for each cell to all buildings: 2500 * 2500 -> TLE
*/
```