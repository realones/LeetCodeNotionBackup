# Number of Closed Islands

#: 1254
Difficult: Medium
Tags: BFS, DFS, Matrix, Union Find
link: https://leetcode.com/problems/number-of-closed-islands/description/
Priority: Pass
Created time: December 24, 2023 5:02 PM
Last edited time: December 24, 2023 5:02 PM
source: googleFreq

Given a 2D `grid` consists of `0s` (land) and `1s` (water).  An *island* is a maximal 4-directionally connected group of `0s` and a *closed island* is an island **totally** (all left, top, right, bottom) surrounded by `1s.`

Return the number of *closed islands*.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png](https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png)

```
Input: grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]
Output: 2
Explanation:
Islands in gray are closed because they are completely surrounded by water (group of 1s).
```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/10/31/sample_4_1610.png](https://assets.leetcode.com/uploads/2019/10/31/sample_4_1610.png)

```
Input: grid = [[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]
Output: 1

```

**Example 3:**

```
Input: grid = [[1,1,1,1,1,1,1],
               [1,0,0,0,0,0,1],
               [1,0,1,1,1,0,1],
               [1,0,1,0,1,0,1],
               [1,0,1,1,1,0,1],
               [1,0,0,0,0,0,1],
               [1,1,1,1,1,1,1]]
Output: 2

```

**Constraints:**

- `1 <= grid.length, grid[0].length <= 100`
- `0 <= grid[i][j] <=1`

```cpp
//cp from votrubac to pass
class Solution {
public:
    int fill(vector<vector<int>>& g, int i, int j) {
        if (i < 0 || j < 0 || i >= g.size() || j >= g[i].size() || g[i][j])
            return 0;
        return (g[i][j] = 1) + fill(g, i + 1, j) + fill(g, i, j + 1) 
            + fill(g, i - 1, j) + fill(g, i, j - 1);
    }
    int closedIsland(vector<vector<int>>& g, int res = 0) {
        for (int i = 0; i < g.size(); ++i)
            for (int j = 0; j < g[i].size(); ++j)
                if (i * j == 0 || i == g.size() - 1 || j == g[i].size() - 1)
                    fill(g, i, j);
        for (int i = 0; i < g.size(); ++i)
            for (int j = 0; j < g[i].size(); ++j)
                res += fill(g, i, j) > 0;
        return res;
    }
};

/*
dfs/bfs to mark all boundary linked land to water

for each cell
    dfs/bfs to find the current size of island
    update res
*/
```