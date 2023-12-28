# Swim in Rising Water

#: 778
Difficult: Hard
Tags: BFS, BS_todo, Graph, PQ
link: https://leetcode.com/problems/swim-in-rising-water/
Priority: Medium
Status: More Solution, toSummarize
Created time: October 6, 2022 1:23 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, googleFreq

You are given an `n x n` integer matrix `grid` where each value `grid[i][j]` represents the elevation at that point `(i, j)`.

The rain starts to fall. At time `t`, the depth of the water everywhere is `t`. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most `t`. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return *the least time until you can reach the bottom right square* `(n - 1, n - 1)` *if you start at the top left square* `(0, 0)`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/06/29/swim1-grid.jpg](https://assets.leetcode.com/uploads/2021/06/29/swim1-grid.jpg)

```
Input: grid = [[0,2],[1,3]]
Output: 3
Explanation:
At time 0, you are in grid location (0, 0).
You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.
You cannot reach point (1, 1) until time 3.
When the depth of water is 3, we can swim anywhere inside the grid.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/06/29/swim2-grid-1.jpg](https://assets.leetcode.com/uploads/2021/06/29/swim2-grid-1.jpg)

```
Input: grid = [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
Output: 16
Explanation: The final route is shown.
We need to wait until time 16 so that (0, 0) and (4, 4) are connected.

```

**Constraints:**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 50`
- `0 <= grid[i][j] < n2`
- Each value `grid[i][j]` is **unique**.

```cpp
class Node{
public:
    int row, col, val;
    Node(int r, int c, int v):row(r),col(c),val(v){}
    bool operator < (const Node& obj) const {
        return val > obj.val;
    }
};

/*
find the lowest wall in the path from 00 to bb
bfs+pq
*/
class Solution {
public:
    int swimInWater(vector<vector<int>>& matrix) {
        int n=matrix.size(); if(n==0) {return 0;}
        
        priority_queue<Node> pq;
        
        //init 00, res0
        int res=0;
        pq.emplace(0,0,matrix[0][0]);
        matrix[0][0]=-1;//visited
        
        //bfs no sz
        vector<int> d{0,1,0,-1,0};
        while(!pq.empty()){
            Node cur=pq.top(); pq.pop();
            int x=cur.row, y=cur.col, h=cur.val;
            //res=max res and cur
            res=max(res,h);
            //if find bb, return res
            if(x==n-1 && y==n-1) return res;

            //find nei, if not visited put into pq
            //for each neighbor
            for(int k=0;k<4;k++){
                int nx=x+d[k];
                int ny=y+d[k+1];
                if(nx<0 || nx>n-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]==-1) continue;
                pq.emplace(nx,ny,matrix[nx][ny]);
                matrix[nx][ny]=-1;
            }
        }
        return res;
    }
};
```