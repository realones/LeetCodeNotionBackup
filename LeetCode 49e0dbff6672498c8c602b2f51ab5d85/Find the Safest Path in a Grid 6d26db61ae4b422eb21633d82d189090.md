# Find the Safest Path in a Grid

#: 2812
Difficult: Medium
Tags: BFS, BS_Val, DFS, Dijkstra, Matrix, Union Find
link: https://leetcode.com/problems/find-the-safest-path-in-a-grid/
Priority: High
Status: More Solution, No Idea At First
Created time: August 5, 2023 11:33 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** 2D matrix `grid` of size `n x n`, where `(r, c)` represents:

- A cell containing a thief if `grid[r][c] = 1`
- An empty cell if `grid[r][c] = 0`

You are initially positioned at cell `(0, 0)`. In one move, you can move to any adjacent cell in the grid, including cells containing thieves.

The **safeness factor** of a path on the grid is defined as the **minimum** manhattan distance from any cell in the path to any thief in the grid.

Return *the **maximum safeness factor** of all paths leading to cell* `(n - 1, n - 1)`*.*

An **adjacent** cell of cell `(r, c)`, is one of the cells `(r, c + 1)`, `(r, c - 1)`, `(r + 1, c)` and `(r - 1, c)` if it exists.

The **Manhattan distance** between two cells `(a, b)` and `(x, y)` is equal to `|a - x| + |b - y|`, where `|val|` denotes the absolute value of val.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/07/02/example1.png](https://assets.leetcode.com/uploads/2023/07/02/example1.png)

```
Input: grid = [[1,0,0],[0,0,0],[0,0,1]]
Output: 0
Explanation: All paths from (0, 0) to (n - 1, n - 1) go through the thieves in cells (0, 0) and (n - 1, n - 1).

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/07/02/example2.png](https://assets.leetcode.com/uploads/2023/07/02/example2.png)

```
Input: grid = [[0,0,1],[0,0,0],[0,0,0]]
Output: 2
Explanation: The path depicted in the picture above has a safeness factor of 2 since:
- The closest cell of the path to the thief at cell (0, 2) is cell (0, 0). The distance between them is | 0 - 0 | + | 0 - 2 | = 2.
It can be shown that there are no other paths with a higher safeness factor.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2023/07/02/example3.png](https://assets.leetcode.com/uploads/2023/07/02/example3.png)

```
Input: grid = [[0,0,0,1],[0,0,0,0],[0,0,0,0],[1,0,0,0]]
Output: 2
Explanation: The path depicted in the picture above has a safeness factor of 2 since:
- The closest cell of the path to the thief at cell (0, 3) is cell (1, 2). The distance between them is | 0 - 1 | + | 3 - 2 | = 2.
- The closest cell of the path to the thief at cell (3, 0) is cell (3, 2). The distance between them is | 3 - 3 | + | 0 - 2 | = 2.
It can be shown that there are no other paths with a higher safeness factor.

```

**Constraints:**

- `1 <= grid.length == n <= 400`
- `grid[i].length == n`
- `grid[i][j]` is either `0` or `1`.
- There is at least one thief in the `grid`.

Solution Dijkstra:

```cpp
using ii=pair<int,int>;
vector<int> dir{-1,0,1,0,-1};

class Solution {
public:
    int maximumSafenessFactor(vector<vector<int>>& matrix) {
        int n=matrix.size();
        //map matrix to matrix of dist (cur cell to neareat 1)
        //multi source bfs to calcu dist to 0s
        queue<ii> q;
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1) {
                    matrix[i][j]=0;
                    q.emplace(i,j);
                }
                else matrix[i][j]=-1;
            }
        }
        int dist=1;
        while(!q.empty()){
            int qSz=q.size();
            for(int i=0; i<qSz; i++){
                auto [x,y] = q.front(); q.pop();
                for(int i=0; i<4; i++){
                    int nx = x + dir[i];
                    int ny = y + dir[i+1];
                    if(nx<0 || nx>n-1 || ny<0 || ny>n-1) continue;
                    if(matrix[nx][ny]==-1) {
                        matrix[nx][ny]=dist;
                        q.emplace(nx,ny);
                    }
                }
            }
            dist++;
        }
        //now we know the max and min, use BS on val,
        //dijkstra
        return dijkstra(matrix, ii(0,0), ii(n-1,n-1));
    }

    //val,x,y
    using dxy = tuple<int,int,int>;
    int dijkstra(vector<vector<int>>& matrix, ii start, ii end)
    {
        int m=matrix.size(); int n=matrix[0].size();
        priority_queue<dxy, vector<dxy>, less<>> pq;
        vector<vector<int>> dist(m,vector<int>(n,-1));

        pq.emplace(matrix[start.first][start.second], start.first, start.second);
        while (!pq.empty())
        {
            auto [d, x, y] = pq.top(); pq.pop();
            if (dist[x][y]!=-1) continue;
            dist[x][y] = d;
            if(x==end.first && y==end.second) break;
            
            for(int i=0; i<4; i++){
                int nx = x + dir[i];
                int ny = y + dir[i+1];
                if(nx<0 || nx>n-1 || ny<0 || ny>n-1) continue;
                int nw = matrix[nx][ny];

                if (dist[nx][ny]!=-1) continue;
                pq.emplace(min(d,nw), nx, ny);
            }
        }
        return dist[end.first][end.second];
    }
};
```

Solution BS:

```cpp
using ii=pair<int,int>;

class Solution {
public:
    vector<int> d{-1,0,1,0,-1};
    
    int maximumSafenessFactor(vector<vector<int>>& matrix) {
        int n=matrix.size();
        //map matrix to matrix of dist (cur cell to neareat 1)
        //multi source bfs to calcu dist to 0s
        queue<ii> q;
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1) {
                    matrix[i][j]=0;
                    q.emplace(i,j);
                }
                else matrix[i][j]=-1;
            }
        }
        int dist=1;
        while(!q.empty()){
            int qSz=q.size();
            for(int i=0; i<qSz; i++){
                auto [x,y] = q.front(); q.pop();
                for(int i=0; i<4; i++){
                    int nx = x + d[i];
                    int ny = y + d[i+1];
                    if(nx<0 || nx>n-1 || ny<0 || ny>n-1) continue;
                    if(matrix[nx][ny]==-1) {
                        matrix[nx][ny]=dist;
                        q.emplace(nx,ny);
                    }
                }
            }
            dist++;
        }
        //now we know the max and min, use BS on val,
        /*
        bs: yyyyyynnnnn, last valid
        validation: for given score, check if there is a path with all cell val >= score
            intui: dfs from n-1,n-1 till touch 0, treat cell<score as wall, see if exist a path from 0,0 to n-1,n-1
        */
        int l=0, r=dist-1;
        while(l<r){
            int m=l+((long long)r-l+1)/2;
            if(!valid(matrix,m)) {r=m-1;}//if not qualify
            else {l=m;}
        }
        return r;
    }
    
    bool valid(vector<vector<int>>& matrix, int minScore){
        int n=matrix.size();
        vector<vector<int>> dp(n,vector<int>(n,-1));
        vector<vector<int>> visited(n,vector<int>(n,0));
        return dfs(matrix,dp,visited,minScore,n-1,n-1);
    }

    vector<int> dx{0,1,0,-1};
    vector<int> dy{1,0,-1,0};
    
    bool dfs(vector<vector<int>> &matrix, vector<vector<int>> &dp, vector<vector<int>>& visited, int minScore, int x, int y){
        visited[x][y]=1;
        if(matrix[x][y]<minScore) return 0;
        if(x==0 && y==0) return 1;

        int n=matrix.size();
        if(dp[x][y]!=-1){return dp[x][y];}        

        int res=0;
        for(int k=0;k<4;k++){
            int nx=x+dx[k];
            int ny=y+dy[k];
            if(nx<0 || nx>n-1 || ny<0 || ny>n-1) continue;
            if(visited[nx][ny]) continue;
            if(dfs(matrix,dp,visited, minScore,nx,ny)) {
                res=true;
                break;
            }
        }

        dp[x][y]=res;
        return dp[x][y];
    }
};

/*
n: 1~400
distance (a, b) -> (x, y) = |a - x| + |b - y|
================================

bfs to get score of each cell
find path (0,0) -> (n-1,n-1) with max of min cell

*/
```