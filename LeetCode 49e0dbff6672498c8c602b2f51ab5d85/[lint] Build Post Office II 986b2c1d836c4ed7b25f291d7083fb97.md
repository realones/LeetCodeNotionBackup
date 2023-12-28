# [lint] Build Post Office II

#: 573
Difficult: Hard
Tags: BFS, Matrix, mbfs_preChkVisited_wSz
link: https://www.lintcode.com/problem/build-post-office-ii
Priority: Low
Status: Next Up
Created time: July 26, 2022 5:37 PM
Last edited time: October 16, 2023 2:30 PM
practicedTimes: 1
source: jiuzhang, 算初BFS

Given a 2D grid, each cell is either a wall `2`, an house `1` or empty `0` (the number zero, one, two), find a place to build a post office so that the sum of the distance from the post office to all the houses is smallest.

Return the smallest sum of distance. Return `-1` if it is not possible.

Example:
**Example 1:**

```
Input：[[0,1,0,0,0],[1,0,0,2,1],[0,1,0,0,0]]
Output：8
Explanation： Placing a post office at (1,1), the distance that post office to all the house sum is smallest.

```

**Example 2:**

```
Input：[[0,1,0],[1,0,1],[0,1,0]]
Output：4
Explanation： Placing a post office at (1,1), the distance that post office to all the house sum is smallest.

```

Challenge

Related Questions:
shortest-distance-from-all-buildings,walls-and-gates,zombie-in-matrix,build-post-office

Tags:
Google,Zenefits,Breadth-first Search

```cpp
// this solution is from house to empty lands
class node {
public:
    node(){}
    node(int xx, int yy, int dist) {
        x = xx;
        y = yy;
        dis = dist;
    }
    int x, y, dis;
};

class Solution {
public:
    /**
     * @param grid a 2D grid
     * @return an integer
     */
    int dx[4] = {1,0,-1,0};
    int dy[4] = {0,1,0,-1};

    bool valid(int nx, int ny, int n, int m, vector<vector<int>>& grid, vector<vector<bool>>& flag) {
        if(0 <= nx && nx < n && 0 <= ny && ny < m) {
            if(grid[nx][ny] == 0 && flag[nx][ny] == false) {
                return  true;
            }
        }
        return false;
    }

    void bfs(node now, vector<vector<int>>& grid, vector<vector<int>>& dis, vector<vector<int>>& visit_num, int n, int m) {
        queue<node> q;
        q.push(now);
        vector<vector<bool> > flag(n, vector<bool>(m)) ;

        while(!q.empty()) {
            now = q.front();
            q.pop();
            visit_num[now.x][now.y]++;

            for(int i = 0; i < 4; i++) {
                int nx = now.x + dx[i];
                int ny = now.y + dy[i];
                if (valid(nx, ny, n, m, grid, flag)) {
                    dis[nx][ny] = dis[nx][ny] + now.dis + 1;
                    flag[nx][ny] = true;
                    q.push(node(nx, ny, now.dis+1));
                }
            }
        }
    }
    int shortestDistance(vector<vector<int>>& grid) {
        // Write your code here
        if(grid.size() == 0)
            return 0;
        int n = grid.size();
        int m = grid[0].size();
        vector<vector<int>> dis(n, vector<int>(m, 0));
        vector<vector<int> > visit_num(n, vector<int>(m)) ;

        int house_num = 0;
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[i].size(); j++)
            if (grid[i][j] == 1) {
                house_num++;
                bfs(node(i,j,0), grid, dis, visit_num, n, m);
            }
        }

        int ans = INT_MAX;
        for (int i = 0; i < grid.size(); i++) {
            for(int j = 0; j < grid[i].size(); j++)
            if (grid[i][j] == 0 && visit_num[i][j] == house_num){
                ans = min(ans, dis[i][j]);
            }
        }

        return ans == INT_MAX ? -1 : ans;
    }
};
```

```cpp
// time out!!!, i think because empty lands is much more than house number
// following bfs is from empty land to outside

class Solution {
public:

    int shortestDistance(vector<vector<int>> &grid) {
        // write your code here
        if(grid.size()==0 || grid[0].size()==0) return -1;
        int res=INT_MAX;// todo should fix to avoid overflow //int_max should be public
        int m=grid.size(), n=grid[0].size();
        int houseNum=0;
        for(auto& r :grid){
            for(auto& v: r){
                if(v==1) houseNum++;
            }
        }
        if(houseNum==0) return 0;

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                res=min(res,bfs(grid,i,j,houseNum));
            }
        }
        return res==INT_MAX?-1:res;
    }

private:
    vector<int> dx{0,1,0,-1};
    vector<int> dy{1,0,-1,0};

    int bfs(vector<vector<int>> &grid, int startI ,int startJ, int totalHouseNum){
        if(!validCell(grid,startI,startJ,false)) return INT_MAX;
        int m=grid.size(), n=grid[0].size();
        vector<vector<bool>> visited(m,vector<bool>(n,false));
        int step=0;
        int totalSteps=0;
        int curHouseNum=0;
        queue<pair<int,int>> q;//todo: rename in future
        q.emplace(startI,startJ);
        visited[startI][startJ]=true;
        while(!q.empty()){
            int qSz=q.size();
            step++;
            for(int i=0;i<qSz;i++){//this layer
                pair<int,int> cur=q.front(); q.pop();
                //handle

                //all next neighbers
                for(int k=0;k<4;k++){//4 to const member
                    int nx=cur.first+dx[k], ny=cur.second+dy[k];
                    if(!validCell(grid,nx,ny,true)||visited[nx][ny]) continue;
                    visited[nx][ny]=true;
                    if(grid[nx][ny]==1) {totalSteps+=step;curHouseNum++;}
                    if(grid[nx][ny]==0) q.emplace(nx,ny);
                }

            }

        }
        return curHouseNum==totalHouseNum? totalSteps:INT_MAX;
    }

    bool validCell(vector<vector<int>> &grid, int i ,int j, bool includeHouse){
        if(i<0 || i>=grid.size() || j<0 || j>=grid[0].size()) return false;
        return (grid[i][j]==0 || includeHouse&&grid[i][j]==1);
    }
};
```