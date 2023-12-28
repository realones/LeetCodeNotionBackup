# Matrix Cells in Distance Order

#: 1030
Difficult: Easy
Tags: BFS, Geometry, Matrix, Sort
link: https://leetcode.com/problems/matrix-cells-in-distance-order/description/
Priority: Low
Status: More Solution
Created time: September 25, 2023 12:54 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given four integers `row`, `cols`, `rCenter`, and `cCenter`. There is a `rows x cols` matrix and you are on the cell with the coordinates `(rCenter, cCenter)`.

Return *the coordinates of all cells in the matrix, sorted by their **distance** from* `(rCenter, cCenter)` *from the smallest distance to the largest distance*. You may return the answer in **any order** that satisfies this condition.

The **distance** between two cells `(r1, c1)` and `(r2, c2)` is `|r1 - r2| + |c1 - c2|`.

**Example 1:**

```
Input: rows = 1, cols = 2, rCenter = 0, cCenter = 0
Output: [[0,0],[0,1]]
Explanation: The distances from (0, 0) to other cells are: [0,1]

```

**Example 2:**

```
Input: rows = 2, cols = 2, rCenter = 0, cCenter = 1
Output: [[0,1],[0,0],[1,1],[1,0]]
Explanation: The distances from (0, 1) to other cells are: [0,1,1,2]
The answer [[0,1],[1,1],[0,0],[1,0]] would also be accepted as correct.

```

**Example 3:**

```
Input: rows = 2, cols = 3, rCenter = 1, cCenter = 2
Output: [[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]
Explanation: The distances from (1, 2) to other cells are: [0,1,1,2,2,3]
There are other answers that would also be accepted as correct, such as [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]].

```

**Constraints:**

- `1 <= rows, cols <= 100`
- `0 <= rCenter < rows`
- `0 <= cCenter < cols`

comment:

why there are geometry and sorting tags? more solutions?

Solution BFS:

```cpp
class Solution {
public:
    vector<int> d{-1,0,1,0,-1};
    using ii=pair<int,int>;

    vector<vector<int>> allCellsDistOrder(int m, int n, int rCenter, int cCenter) { //100*100
        vector<vector<int>> res;

        vector<vector<int>> visited(m, vector<int>(n,0));
        queue<ii> q;
        visited[rCenter][cCenter] = 1;
        q.emplace(rCenter,cCenter);

        while(!q.empty()){
            auto [x,y] = q.front(); q.pop();
            res.push_back({x,y});
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(visited[nx][ny]!=1) {
                    visited[nx][ny]=1;
                    q.emplace(nx,ny);
                }
            }
        }
        return res;
    }
}
```