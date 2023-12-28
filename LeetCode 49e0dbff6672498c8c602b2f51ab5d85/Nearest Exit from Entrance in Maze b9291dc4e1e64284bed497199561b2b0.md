# Nearest Exit from Entrance in Maze

#: 1926
Difficult: Medium
Tags: BFS, Graph, Matrix
link: https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/
Priority: Pass
Created time: October 23, 2022 12:22 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, googleFreq

You are given an `m x n` matrix `maze` (**0-indexed**) with empty cells (represented as `'.'`) and walls (represented as `'+'`). You are also given the `entrance` of the maze, where `entrance = [entrancerow, entrancecol]` denotes the row and column of the cell you are initially standing at.

In one step, you can move one cell **up**, **down**, **left**, or **right**. You cannot step into a cell with a wall, and you cannot step outside the maze. Your goal is to find the **nearest exit** from the `entrance`. An **exit** is defined as an **empty cell** that is at the **border** of the `maze`. The `entrance` **does not count** as an exit.

Return *the **number of steps** in the shortest path from the* `entrance` *to the nearest exit, or* `-1` *if no such path exists*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/06/04/nearest1-grid.jpg](https://assets.leetcode.com/uploads/2021/06/04/nearest1-grid.jpg)

```
Input: maze = [["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]
Output: 1
Explanation: There are 3 exits in this maze at [1,0], [0,2], and [2,3].
Initially, you are at the entrance cell [1,2].
- You can reach [1,0] by moving 2 steps left.
- You can reach [0,2] by moving 1 step up.
It is impossible to reach [2,3] from the entrance.
Thus, the nearest exit is [0,2], which is 1 step away.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/06/04/nearesr2-grid.jpg](https://assets.leetcode.com/uploads/2021/06/04/nearesr2-grid.jpg)

```
Input: maze = [["+","+","+"],[".",".","."],["+","+","+"]], entrance = [1,0]
Output: 2
Explanation: There is 1 exit in this maze at [1,2].
[1,0] does not count as an exit since it is the entrance cell.
Initially, you are at the entrance cell [1,0].
- You can reach [1,2] by moving 2 steps right.
Thus, the nearest exit is [1,2], which is 2 steps away.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/06/04/nearest3-grid.jpg](https://assets.leetcode.com/uploads/2021/06/04/nearest3-grid.jpg)

```
Input: maze = [[".","+"]], entrance = [0,0]
Output: -1
Explanation: There are no exits in this maze.

```

**Constraints:**

- `maze.length == m`
- `maze[i].length == n`
- `1 <= m, n <= 100`
- `maze[i][j]` is either `'.'` or `'+'`.
- `entrance.length == 2`
- `0 <= entrancerow < m`
- `0 <= entrancecol < n`
- `entrance` will always be an empty cell.

```cpp
class Solution {
public:
    typedef pair<int,int> Block;
    vector<int> d{-1,0,1,0,-1};
    
    int nearestExit(vector<vector<char>>& maze, vector<int>& entrance) {
        int m=maze.size(); if(m==0) {return -1;}
        int n=maze[0].size(); if(n==0) {return -1;}
        int res=0;
        bool firstCell=true;
        //bfs
        queue<Block> q;
        maze[entrance[0]][entrance[1]]='v';//mark as visited
        q.emplace(entrance[0],entrance[1]);
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                Block cur=q.front(); q.pop();
                if(cur.first==0 || cur.first==m-1 || cur.second==0 || cur.second==n-1){
                    if(!firstCell)
                        return res;
                }
                firstCell=false;
                int x=cur.first, y=cur.second;
                for(int k=0;k<4;k++){
                    int nx=x+d[k];
                    int ny=y+d[k+1];
                    if(nx<0 || nx>m-1 || ny<0 ||ny>n-1) continue;
                    if(maze[nx][ny]=='.'){
                        maze[nx][ny]='v';
                        q.emplace(nx,ny);
                    }
                }
            }
            res++;
        }
        return -1;
    }
};
```