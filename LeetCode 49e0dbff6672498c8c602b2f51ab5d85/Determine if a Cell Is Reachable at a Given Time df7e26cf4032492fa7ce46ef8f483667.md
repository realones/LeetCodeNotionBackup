# Determine if a Cell Is Reachable at a Given Time

#: 2849
Difficult: Medium
Tags: Math
link: https://leetcode.com/problems/determine-if-a-cell-is-reachable-at-a-given-time
Priority: Low
Created time: November 7, 2023 6:59 PM
Last edited time: November 7, 2023 7:03 PM
source: leetcode_daily

You are given four integers `sx`, `sy`, `fx`, `fy`, and a **non-negative** integer `t`.

In an infinite 2D grid, you start at the cell `(sx, sy)`. Each second, you **must** move to any of its adjacent cells.

Return `true` *if you can reach cell* `(fx, fy)` *after **exactly*** `t` ***seconds***, *or* `false` *otherwise*.

A cell's **adjacent cells** are the 8 cells around it that share at least one corner with it. You can visit the same cell several times.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/08/05/example2.svg](https://assets.leetcode.com/uploads/2023/08/05/example2.svg)

```
Input: sx = 2, sy = 4, fx = 7, fy = 7, t = 6
Output: true
Explanation: Starting at cell (2, 4), we can reach cell (7, 7) in exactly 6 seconds by going through the cells depicted in the picture above.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/08/05/example1.svg](https://assets.leetcode.com/uploads/2023/08/05/example1.svg)

```
Input: sx = 3, sy = 1, fx = 7, fy = 3, t = 3
Output: false
Explanation: Starting at cell (3, 1), it takes at least 4 seconds to reach cell (7, 3) by going through the cells depicted in the picture above. Hence, we cannot reach cell (7, 3) at the third second.

```

**Constraints:**

- `1 <= sx, sy, fx, fy <= 109`
- `0 <= t <= 109`

Solution O(1)

```jsx
using ii=pair<int,int>;
vector<int> dx{-1,-1,-1,0,0,1,1,1};
vector<int> dy{-1,0,1,-1,1,-1,0,1};

class Solution {
public:
    //x/y: 1~1e9
    //t: 0~1e9
    bool isReachableAtTime(int sx, int sy, int fx, int fy, int t) {
        //corner case
        if(sx==fx && sy==fy) return t!=1;
        //handling
        int xDiff=abs(sx-fx);
        int yDiff=abs(sy-fy);
        int shortestDist=max(xDiff,yDiff);

        return shortestDist<=t;
    }
};

/*
bfs to find shortest path, return if <=t
*/
```

Solution TLE:

```jsx
using ii=pair<int,int>;
vector<int> dx{-1,-1,-1,0,0,1,1,1};
vector<int> dy{-1,0,1,-1,1,-1,0,1};

class Solution {
public:
    //x/y: 1~1e9
    //t: 0~1e9
    bool isReachableAtTime(int sx, int sy, int fx, int fy, int t) {
        //corner case
        if(sx==fx && sy==fy) return t!=1;
        //handling
        int shortestDist=bfs({sx,sy}, {fx,fy});
        return shortestDist<=t;
    }

    int bfs(ii start, ii finish){
        unordered_map<int,unordered_map<int,bool>> visited;
        queue<ii> q;

        visited[start.first][start.second] = true;//mark as visited
        q.push(start);

        int dist=0;
        while(!q.empty()){
            int qSz=q.size();
            for(int i=0; i<qSz; i++){
                auto [x,y] = q.front(); q.pop();
                if(ii{x,y} == finish) return dist;
                for(int i=0; i<8; i++){
                    int nx = x + dx[i];
                    int ny = y + dy[i];
                    if(!visited[nx][ny]) {
                        visited[nx][ny]=true;
                        q.emplace(nx,ny);
                    }
                }
            }
            dist++;
        }
        return -1;
    }
};

/*
bfs to find shortest path, return if <=t
*/
```