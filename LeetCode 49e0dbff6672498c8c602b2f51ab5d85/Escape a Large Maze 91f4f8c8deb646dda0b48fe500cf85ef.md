# Escape a Large Maze

#: 1036
Difficult: Hard
Tags: Array, BFS, Brainteaser, DFS, Hash
link: https://leetcode.com/problems/escape-a-large-maze/
Priority: Medium
Status: No Idea At First
Created time: December 19, 2023 8:17 PM
Last edited time: December 19, 2023 8:18 PM
source: googleFreq

There is a 1 million by 1 million grid on an XY-plane, and the coordinates of each grid square are `(x, y)`.

We start at the `source = [sx, sy]` square and want to reach the `target = [tx, ty]` square. There is also an array of `blocked` squares, where each `blocked[i] = [xi, yi]` represents a blocked square with coordinates `(xi, yi)`.

Each move, we can walk one square north, east, south, or west if the square is **not** in the array of `blocked` squares. We are also not allowed to walk outside of the grid.

Return `true` *if and only if it is possible to reach the* `target` *square from the* `source` *square through a sequence of valid moves*.

**Example 1:**

```
Input: blocked = [[0,1],[1,0]], source = [0,0], target = [0,2]
Output: false
Explanation: The target square is inaccessible starting from the source square because we cannot move.
We cannot move north or east because those squares are blocked.
We cannot move south or west because we cannot go outside of the grid.

```

**Example 2:**

```
Input: blocked = [], source = [0,0], target = [999999,999999]
Output: true
Explanation: Because there are no blocked cells, it is possible to reach the target square.

```

**Constraints:**

- `0 <= blocked.length <= 200`
- `blocked[i].length == 2`
- `0 <= xi, yi < 106`
- `source.length == target.length == 2`
- `0 <= sx, sy, tx, ty < 106`
- `source != target`
- It is guaranteed that `source` and `target` are not blocked.

```cpp
vector<int> d{-1,0,1,0,-1};
using ii=pair<int,int>;

class Solution {
public:
    bool isEscapePossible(vector<vector<int>>& blocked, vector<int>& source, vector<int>& target) {
        unordered_map<int,unordered_map<int,bool>> visited;
        for(auto& b: blocked){
            visited[b[0]][b[1]]=true;
        }
        bool s=bfs(visited, {source[0],source[1]}, {target[0],target[1]});
        bool t=bfs(visited, {target[0],target[1]}, {source[0],source[1]});
        return s&&t;
    }

    //return if root can skip the surrounding or meet end
    bool bfs(unordered_map<int,unordered_map<int,bool>> visited, ii root, ii end){
        int m=1e6, n=1e6;
        queue<ii> q;

        visited[root.first][root.second]=true;
        q.push(root);

        int cnt=0;
        while(!q.empty()){
            auto [x,y] = q.front(); q.pop();
            if(end==ii{x,y}) return true;
            cnt++;
            if(cnt>19900) return true;
            for(int i=0; i<4; i++){
                int nx = x + d[i];
                int ny = y + d[i+1];
                if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                if(!visited[nx][ny]) {
                    visited[nx][ny]=true;
                    q.emplace(nx,ny);
                }
            }
        }
        return false;
    }
};

/*
source: [x,y]
target: [x,y]
blocked: [(x,y)]
    len: 200
x,y: ==1e6, pay attention it is ==1e6
================================
if there is a way: dfs/bfs, return if search from source can reach target: TLE

block cnt is small: 200
check if it split source and target

assume the maximum area surrounded by blockCell is X:
X is 19900:
0th      _________________________
         |O O O O O O O X
         |O O O O O O X
         |O O O O O X
         |O O O O X
         .O O O X
         .O O X
         .O X
200th    |X

0+1+2+..+199 = 19900

from source, check if can search >X times, or find target
from target, check if can search >X times, or find source

*/
```