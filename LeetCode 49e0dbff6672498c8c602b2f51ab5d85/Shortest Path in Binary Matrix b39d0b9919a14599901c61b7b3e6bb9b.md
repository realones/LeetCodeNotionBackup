# Shortest Path in Binary Matrix

#: 1091
Difficult: Medium
Tags: BFS, Matrix
link: https://leetcode.com/problems/shortest-path-in-binary-matrix/
Priority: Low
Created time: May 31, 2023 8:41 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an `n x n` binary matrix `grid`, return *the length of the shortest **clear path** in the matrix*. If there is no clear path, return `-1`.

A **clear path** in a binary matrix is a path from the **top-left** cell (i.e., `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)`) such that:

- All the visited cells of the path are `0`.
- All the adjacent cells of the path are **8-directionally** connected (i.e., they are different and they share an edge or a corner).

The **length of a clear path** is the number of visited cells of this path.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/02/18/example1_1.png](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

```
Input: grid = [[0,1],[1,0]]
Output: 2

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/02/18/example2_1.png](https://assets.leetcode.com/uploads/2021/02/18/example2_1.png)

```
Input: grid = [[0,0,0],[1,1,0],[1,1,0]]
Output: 4

```

**Example 3:**

```
Input: grid = [[1,0,0],[1,1,0],[1,1,0]]
Output: -1

```

**Constraints:**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 100`
- `grid[i][j] is 0 or 1`

```cpp
class Solution {
public:
    vector<int> dx{-1,-1,-1,0,0,1,1,1};
    vector<int> dy{-1,0,1,-1,1,-1,0,1};
    typedef pair<int, int> Block;
    
    int shortestPathBinaryMatrix(vector<vector<int>>& matrix) {
        int m=matrix.size(); 
        int n=matrix[0].size();
        if(matrix[0][0]!=0 || matrix.back().back()!=0) return -1;
        queue<Block> q;

        matrix[0][0] = 2;//mark as visited
        q.push({0,0});

        std::cout << "aaaa" << std::endl;
        int step=1;
        while(!q.empty()){
            int qSz=q.size();
            for(int i=0; i<qSz; i++){
                Block cur = q.front(); q.pop();
                int x=cur.first, y=cur.second;
                if(x==m-1 && y==n-1) {
                    return step;
                 }
                
                for(int i=0; i<8; i++){
                    int nx = x + dx[i];
                    int ny = y + dy[i];
                    if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                    if(matrix[nx][ny]==0) {
                        matrix[nx][ny]=2;
                        q.push({nx,ny});
                    }
                }
            }
            step++;
        }
        return -1;
    }
};
```