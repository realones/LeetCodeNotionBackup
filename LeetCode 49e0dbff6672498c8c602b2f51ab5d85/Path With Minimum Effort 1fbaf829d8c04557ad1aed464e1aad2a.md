# Path With Minimum Effort

#: 1631
Difficult: Medium
Tags: BFS, BS_todo, DFS, DP, Dijkstra, Graph, Matrix, PQ, Union Find, search
link: https://leetcode.com/problems/path-with-minimum-effort/description/
Priority: High
Status: More Solution, No Idea At First
Created time: June 27, 2023 11:22 PM
Last edited time: December 15, 2023 10:23 PM
source: HuaHua, microsoftFreq

You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (i.e., **0-indexed**). You can move **up**, **down**, **left**, or **right**, and you wish to find a route that requires the minimum **effort**.

A route's **effort** is the **maximum absolute difference** in heights between two consecutive cells of the route.

Return *the minimum **effort** required to travel from the top-left cell to the bottom-right cell.*

**Example 1:**

![https://assets.leetcode.com/uploads/2020/10/04/ex1.png](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)

```
Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2
Explanation: The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/10/04/ex2.png](https://assets.leetcode.com/uploads/2020/10/04/ex2.png)

```
Input: heights = [[1,2,3],[3,8,4],[5,3,5]]
Output: 1
Explanation: The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].

```

**Example 3:**

![https://assets.leetcode.com/uploads/2020/10/04/ex3.png](https://assets.leetcode.com/uploads/2020/10/04/ex3.png)

```
Input: heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
Output: 0
Explanation: This route does not require any effort.

```

**Constraints:**

- `rows == heights.length`
- `columns == heights[i].length`
- `1 <= rows, columns <= 100`
- `1 <= heights[i][j] <= 106`

comment: 一开始想到了Dijkstra，但是理解不够，梳理思路时候想成了从matrix.bb的top down recursion思路了，后来发现不管是bfs还是dijkstra都是把cur的nei作为未来的可能性放到q/pq中。

```cpp
class Solution {
public:
    int minimumEffortPath(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        return dijkstra(matrix, {0,0}, {m-1,n-1});
    }

    //val,x,y
    using ii=pair<int,int>;
    using dxy = tuple<int,int,int>;
    vector<int> dir={-1,0,1,0,-1};
    int dijkstra(vector<vector<int>>& matrix, ii start, ii end)
    {
        int m=matrix.size(); int n=matrix[0].size();
        priority_queue<dxy, vector<dxy>, greater<>> pq;
        vector<vector<int>> dist(m,vector<int>(n,-1));

        pq.emplace(0, start.first, start.second);
        while (!pq.empty())
        {
            auto [d, x, y] = pq.top(); pq.pop();
            if (dist[x][y]!=-1) continue;
            dist[x][y] = d;
            if(x==end.first && y==end.second) break;
            
            for(int i=0; i<4; i++){
                int nx = x + dir[i];
                int ny = y + dir[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if (dist[nx][ny]!=-1) continue;
                int w = matrix[x][y];
                int nw = matrix[nx][ny];
                int nd = max(d, abs(w-nw));
                pq.emplace(nd, nx, ny);
            }
        }
        return dist[end.first][end.second];
    }
};
```