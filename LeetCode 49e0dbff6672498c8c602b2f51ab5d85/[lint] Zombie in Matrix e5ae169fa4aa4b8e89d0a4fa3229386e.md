# [lint] Zombie in Matrix

#: 598
Difficult: Medium
Tags: BFS, Matrix, mbfs_preChkVisited_wSz
link: https://www.lintcode.com/problem/zombie-in-matrix
Priority: Medium
Created time: July 26, 2022 5:37 PM
Last edited time: October 16, 2023 3:45 PM
practicedTimes: 1
source: jiuzhang, 算初BFS

Given a 2D grid, each cell is either a wall `2`, a zombie `1` or people `0` (the number zero, one, two).Zombies can turn the nearest people(up/down/left/right) into zombies every day, but can not through wall. How long will it take to turn all people into zombies? Return `-1` if can not turn all people into zombies.

Example:
Example 1:

```
Input:
[[0,1,2,0,0],
[1,0,0,2,1],
[0,1,0,0,0]]
Output:
2

```

Example 2:

```
Input:
[[0,0,0],
[0,0,0],
[0,0,1]]
Output:
4

```

Challenge

Related Questions:
build-post-office-ii

图的遍历(层级遍历) - size = queue.size()

```cpp
class Solution {
public:
    /**
     * @param grid: a 2D integer grid
     * @return: an integer
     */
    typedef pair<int,int> Block;

    vector<int> d{-1,0,1,0,-1};

    int zombie(vector<vector<int>> &matrix) {
        // write your code here
        int m=matrix.size(); if(m==0) {return -1;}
        int n=matrix[0].size(); if(n==0) {return -1;}
        int res=0;
        queue<Block> q;
        //init
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1) q.push({i,j});
            }
        }

        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                Block cur = q.front(); q.pop();
                int x=cur.first, y=cur.second;
                for(int k=0;k<4;k++){
                    int nx=x+d[k];
                    int ny=y+d[k+1];
                    if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                    if(matrix[nx][ny]==0){
                        matrix[nx][ny]=1;
                        q.push({nx,ny});
                    }
                }
                res++;
            }
        }

        //if not possible
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
               if(matrix[i][j]==0) return -1;
            }
        }
        return res-1;
    }
};
```