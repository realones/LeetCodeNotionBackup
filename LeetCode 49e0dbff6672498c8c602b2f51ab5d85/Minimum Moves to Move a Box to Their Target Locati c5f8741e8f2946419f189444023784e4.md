# Minimum Moves to Move a Box to Their Target Location

#: 1263
Difficult: Hard
Tags: BFS, Graph, Matrix
link: https://leetcode.com/problems/minimum-moves-to-move-a-box-to-their-target-location/
Priority: High
Status: HardToImplement
Created time: June 28, 2023 12:09 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

A storekeeper is a game in which the player pushes boxes around in a warehouse trying to get them to target locations.

The game is represented by an `m x n` grid of characters `grid` where each element is a wall, floor, or box.

Your task is to move the box `'B'` to the target position `'T'` under the following rules:

- The character `'S'` represents the player. The player can move up, down, left, right in `grid` if it is a floor (empty cell).
- The character `'.'` represents the floor which means a free cell to walk.
- The character`'#'`represents the wall which means an obstacle (impossible to walk there).
- There is only one box `'B'` and one target cell `'T'` in the `grid`.
- The box can be moved to an adjacent free cell by standing next to the box and then moving in the direction of the box. This is a **push**.
- The player cannot walk through the box.

Return *the minimum number of **pushes** to move the box to the target*. If there is no way to reach the target, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/11/06/sample_1_1620.png](https://assets.leetcode.com/uploads/2019/11/06/sample_1_1620.png)

```
Input: grid = [["#","#","#","#","#","#"],
               ["#","T","#","#","#","#"],
               ["#",".",".","B",".","#"],
               ["#",".","#","#",".","#"],
               ["#",".",".",".","S","#"],
               ["#","#","#","#","#","#"]]
Output: 3
Explanation: We return only the number of times the box is pushed.
```

**Example 2:**

```
Input: grid = [["#","#","#","#","#","#"],
               ["#","T","#","#","#","#"],
               ["#",".",".","B",".","#"],
               ["#","#","#","#",".","#"],
               ["#",".",".",".","S","#"],
               ["#","#","#","#","#","#"]]
Output: -1

```

**Example 3:**

```
Input: grid = [["#","#","#","#","#","#"],
               ["#","T",".",".","#","#"],
               ["#",".","#","B",".","#"],
               ["#",".",".",".",".","#"],
               ["#",".",".",".","S","#"],
               ["#","#","#","#","#","#"]]
Output: 5
Explanation: push the box down, left, left, up and up.

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 20`
- `grid` contains only characters `'.'`, `'#'`, `'S'`, `'T'`, or `'B'`.
- There is only one character `'S'`, `'B'`, and `'T'` in the `grid`.

comment: 居然靠自己一遍想通然后写过了，思路是BFS加上考虑玩家的位置，所以要把box和player绑定在一起做bfs

implementation有点复杂

```cpp
using ii=pair<int,int>;
using box_player=tuple<int,int,int,int>;//bx,by,px,py
vector<int> d{-1,0,1,0,-1};

class Solution {
public:
    int minPushBox(vector<vector<char>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        ii box, player, target;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]=='B') box={i,j};
                if(matrix[i][j]=='T') target={i,j};//use else if
                if(matrix[i][j]=='S') player={i,j};
            }
        }
        box_player start{box.first,box.second,player.first,player.second};
        auto [bx,by,px,py]=start;
    
        queue<box_player> q;
        //not use visited to just mark box position, since we can have box in same position but player in diff push position
        vector<vector<vector<vector<int>>>> visited(m,vector<vector<vector<int>>>(n,vector<vector<int>>(m,vector<int>(n,0))));
        q.push(start);
        visited[bx][by][px][py]=1;//px py is not the best ready to push postition, check if need to put multiple start with avaliable ready to push positions, seems not required
        int step=0;

        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                auto [bx,by,px,py] = q.front(); q.pop();
                if(bx==target.first && by==target.second) return step;
                for(int i=0; i<4; i++){
                    int nbx = bx + d[i];
                    int nby = by + d[i+1];
                    int npx = bx - d[i];
                    int npy = by - d[i+1];
                    if(nbx<0 || nbx>m-1 || nby<0 || nby>n-1) continue;//range
                    if(npx<0 || npx>m-1 || npy<0 || npy>n-1) continue;//range
                    if(matrix[nbx][nby]=='#') continue;//wall
                    if(matrix[npx][npy]=='#') continue;//wall
                    if(visited[nbx][nby][npx][npy]) continue;//visited
                    if(!reachable(matrix,{px,py},{npx,npy},{bx,by})) continue;//use dp
                    visited[nbx][nby][npx][npy]=1;//mark as visited
                    q.emplace(nbx,nby,bx,by);//move player to pre box postion
                }
            }
            step++;
        }
        return -1;
    }

    //not use ref for now
    bool reachable(vector<vector<char>> matrix, ii start, ii end, ii box){
        int m=matrix.size(); 
        int n=matrix[0].size();
        matrix[box.first][box.second] = '#';

        queue<ii> q;
        matrix[start.first][start.second] = '#';//mark as visited
        q.push(start);

        while(!q.empty()){
            auto [x,y] = q.front(); q.pop();
            if(x==end.first && y==end.second) return true;
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(matrix[nx][ny]=='#') continue;
                matrix[nx][ny]='#';
                q.emplace(nx,ny);
            }
        }
        return false;
    }
};

/*
box: B
target postition: T
player: S
empty cell: .
wall: #
only push operation

return: min move of *pushes* to push to target, fallback is -1
================================
for people:(not need to calcu, can use to do validation) 
    for each box postition
        we can calcu from cur position to ready to push position, -> bfs

for box move: -> bfs
    we need to know each box position from B to T, the shortest posible solution

we can from B bfs to T, if invalid, skip, find the valid path
    valid: cur -> nxt, the player can reach the push position of cur 

*/
```