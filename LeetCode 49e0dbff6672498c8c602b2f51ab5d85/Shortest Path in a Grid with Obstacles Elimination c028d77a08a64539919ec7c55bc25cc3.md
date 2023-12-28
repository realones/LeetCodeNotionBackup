# Shortest Path in a Grid with Obstacles Elimination

#: 1293
Difficult: Hard
Tags: BFS, Matrix
link: https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/solution/
Priority: High
Status: Dig More, More Solution, Next Up
Created time: October 2, 2022 5:33 PM
Last edited time: November 17, 2023 1:59 AM
practicedTimes: 1
source: googleFreq, ticktokFreq

You are given an `m x n` integer matrix `grid` where each cell is either `0` (empty) or `1` (obstacle). You can move up, down, left, or right from and to an empty cell in **one step**.

Return *the minimum number of **steps** to walk from the upper left corner* `(0, 0)` *to the lower right corner* `(m - 1, n - 1)` *given that you can eliminate **at most*** `k` *obstacles*. If it is not possible to find such walk return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/09/30/short1-grid.jpg](https://assets.leetcode.com/uploads/2021/09/30/short1-grid.jpg)

```
Input: grid = [[0,0,0],[1,1,0],[0,0,0],[0,1,1],[0,0,0]], k = 1
Output: 6
Explanation:
The shortest path without eliminating any obstacle is 10.
The shortest path with one obstacle elimination at position (3,2) is 6. Such path is (0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) ->(3,2) -> (4,2).

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/09/30/short2-grid.jpg](https://assets.leetcode.com/uploads/2021/09/30/short2-grid.jpg)

```
Input: grid = [[0,1,1],[1,1,1],[1,0,0]], k = 1
Output: -1
Explanation: We need to eliminate at least two obstacles to find such a walk.

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 40`
- `1 <= k <= m * n`
- `grid[i][j]` is either `0` **or** `1`.
- `grid[0][0] == grid[m - 1][n - 1] == 0`

comment:

seems the bfs works because if we break a wall and step into the cell, we will never go back to the same cell in the future, because it is a waste of steps.

as a result, you can mark visited[position][k] is always marked in the shortest steps

Solution - BFS

```cpp
using iii=tuple<int,int,int>;
vector<int> d{-1,0,1,0,-1};

class Solution {
public:
    int shortestPath(vector<vector<int>>& matrix, int K) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        vector<vector<vector<int>>> visited(m,vector<vector<int>>(n,vector<int>(K+1,0)));

        queue<iii> q;

        visited[0][0][0] = 1;//mark as visited
        q.push({0,0,0});

        int step=0;
        while(!q.empty()){
            int qSz=q.size();
            for(int i=0; i<qSz; i++){
                auto [x,y,k] = q.front(); q.pop();
                if(x==m-1 && y==n-1) return step;
                for(int i=0; i<4; i++){
                    int nx = x + d[i];
                    int ny = y + d[i+1];
                    if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                    int nk=k+matrix[nx][ny];
                    if(nk>K) continue;
                    if(visited[nx][ny][nk]==0) {
                        visited[nx][ny][nk]=1;
                        q.emplace(nx,ny,nk);
                    }
                }
            }
            step++;
        }
        return -1;
    }
};

/*
bfs:
visited(position)

f(cur,k)
    ob
        f(nei,k-1)
    no_ob
        f(nei,k)

goto same position, larger k suppress smaller k

dp(position, k)

return: 
    dp(target, any).min
*/
```

Solution - DFS, failed last test case, because dfs might calculated longer result first.

```cpp
class Solution {
public:
    int shortestPath(vector<vector<int>>& matrix, int k) {
        int m=matrix.size(); if(m==0) {return -1;}
        int n=matrix[0].size(); if(n==0) {return -1;}
        //-2: not calculated yet //-1 impossible
        vector<vector<vector<int>>> f(m,vector<vector<int>>(n, vector<int>(k+1,-2)));
        //init
        f[0][0][0]=0;
        int res=INT_MAX;
        for(int i=0; i<k+1; i++){
            int cur=helper(matrix, f, m-1,n-1,i,0);
            if(cur==-1) continue;
            res=min(res,cur);
        }
        return res==INT_MAX? -1: res;
    }
    
    vector<int> d{0,1,0,-1,0};
    
    int helper(vector<vector<int>>& matrix, vector<vector<vector<int>>>& f, int x, int y, int z, int q){
        std::cout << string(q,' ') << "xyz: " << x << " " << y << " "<< z<< std::endl;
        int m=f.size(), n=f[0].size(), o=f[0][0].size();
        
        if(f[x][y][z]!=-2) return f[x][y][z];
        f[x][y][z]=-3;//visited
        //for each neighbor
        int fxyz=INT_MAX;
        for(int k=0;k<4;k++){
            int nx=x+d[k];
            int ny=y+d[k+1];
            int nz=matrix[x][y]?z-1:z;
            if(nx<0 || nx>m-1 || ny<0 || ny>n-1 ||nz<0 ||nz>o-1) continue;
            if(f[nx][ny][nz]==-3) continue;
            int pre=helper(matrix,f,nx,ny,nz,q+1);
            if(pre==-1) continue;
            fxyz=min(fxyz, pre+1);
        }
        f[x][y][z]=fxyz==INT_MAX?-1:fxyz;
        std::cout << string(q,' ') << "xyz: " << x << " " << y << " "<< z<< "  value: " << f[x][y][z]<<std::endl;
        return f[x][y][z];
    }
};
```