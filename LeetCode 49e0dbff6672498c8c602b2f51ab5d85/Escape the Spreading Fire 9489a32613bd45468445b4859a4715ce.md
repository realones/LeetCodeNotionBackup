# Escape the Spreading Fire

#: 2258
Difficult: Hard
Tags: BFS, BS_todo, Matrix
link: https://leetcode.com/problems/escape-the-spreading-fire
Priority: High
Status: HardToImplement, checkBetterSolution, toRevisit
Created time: November 10, 2023 12:03 AM
Last edited time: November 10, 2023 12:05 AM
source: leetcode_daily

You are given a **0-indexed** 2D integer array `grid` of size `m x n` which represents a field. Each cell has one of three values:

- `0` represents grass,
- `1` represents fire,
- `2` represents a wall that you and fire cannot pass through.

You are situated in the top-left cell, `(0, 0)`, and you want to travel to the safehouse at the bottom-right cell, `(m - 1, n - 1)`. Every minute, you may move to an **adjacent** grass cell. **After** your move, every fire cell will spread to all **adjacent** cells that are not walls.

Return *the **maximum** number of minutes that you can stay in your initial position before moving while still safely reaching the safehouse*. If this is impossible, return `-1`. If you can **always** reach the safehouse regardless of the minutes stayed, return `109`.

Note that even if the fire spreads to the safehouse immediately after you have reached it, it will be counted as safely reaching the safehouse.

A cell is **adjacent** to another cell if the former is directly north, east, south, or west of the latter (i.e., their sides are touching).

**Example 1:**

![https://assets.leetcode.com/uploads/2022/03/10/ex1new.jpg](https://assets.leetcode.com/uploads/2022/03/10/ex1new.jpg)

```
Input: grid = [[0,2,0,0,0,0,0],[0,0,0,2,2,1,0],[0,2,0,0,1,2,0],[0,0,2,2,2,0,2],[0,0,0,0,0,0,0]]
Output: 3
Explanation: The figure above shows the scenario where you stay in the initial position for 3 minutes.
You will still be able to safely reach the safehouse.
Staying for more than 3 minutes will not allow you to safely reach the safehouse.
```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/03/10/ex2new2.jpg](https://assets.leetcode.com/uploads/2022/03/10/ex2new2.jpg)

```
Input: grid = [[0,0,0,0],[0,1,2,0],[0,2,0,0]]
Output: -1
Explanation: The figure above shows the scenario where you immediately move towards the safehouse.
Fire will spread to any cell you move towards and it is impossible to safely reach the safehouse.
Thus, -1 is returned.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2022/03/10/ex3new.jpg](https://assets.leetcode.com/uploads/2022/03/10/ex3new.jpg)

```
Input: grid = [[0,0,0],[2,2,0],[1,2,0]]
Output: 1000000000
Explanation: The figure above shows the initial grid.
Notice that the fire is contained by walls and you will always be able to safely reach the safehouse.
Thus, 109 is returned.

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 300`
- `4 <= m * n <= 2 * 104`
- `grid[i][j]` is either `0`, `1`, or `2`.
- `grid[0][0] == grid[m - 1][n - 1] == 0`

comment:

hard to implement, the corner case is very hard to handle, i used a stupid way which is to run the same algorithm twice.

todo: check better solution

[https://leetcode.com/problems/escape-the-spreading-fire/solutions/2016835/no-bs/](https://leetcode.com/problems/escape-the-spreading-fire/solutions/2016835/no-bs/)

Solution:

```jsx
using ii=pair<int,int>;
vector<int> d{-1,0,1,0,-1};

class Solution {
public:
    int maximumMinutes(vector<vector<int>>& matrix) {
        int w=helper(matrix,0);
        if(w==1e9 || w<=0) return w;
        if(helper(matrix,w)!=-1) return w;//if can pass with waiting time
        return w-1;
    }
    int helper(vector<vector<int>> matrix, int waitTime) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0

        //init people
        queue<ii> qp;//people
        matrix[0][0] = 3;//mark as people visited
        qp.push({0,0});

        //init fire
        queue<ii> qf;//fire
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==1) qf.push({i,j});
            }
        }

        int stepP=0, resP=0, w=0;
        int stepF=0, resF=0;
        bool endP=false, endF=false;
        while(!qp.empty() || !qf.empty()){
            //people move
            if(w<waitTime) w++;
            else{
                int qpSz=qp.size();
                for(int i=0; i<qpSz; i++){
                    auto [x,y] = qp.front(); qp.pop();
                    if(ii{x,y}==ii{m-1,n-1}) {resP=stepP; endP=true;}//touch end
                    if(matrix[x][y]!=3) continue;//if on fire, then no longer valid
                    for(int i=0; i<4; i++){
                        int nx = x + d[i];
                        int ny = y + d[i+1];
                        if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                        if(matrix[nx][ny]==0) {//only goto non-visited grass
                            matrix[nx][ny]=3;
                            qp.emplace(nx,ny);
                        }
                    }
                }
                stepP++;
            }

            //fire move
            int qfSz=qf.size();
            for(int i=0; i<qfSz; i++){
                auto [x,y] = qf.front(); qf.pop();
                if(ii{x,y}==ii{m-1,n-1}) {resF=stepF; endF=true;}//touch end
                for(int i=0; i<4; i++){
                    int nx = x + d[i];
                    int ny = y + d[i+1];
                    if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
                    if(matrix[nx][ny]==0 || matrix[nx][ny]==3) {//goto grass or people visited
                        matrix[nx][ny]=1;
                        qf.emplace(nx,ny);
                    }
                }
            }
            stepF++;
        }

        if(!endP) return -1;
        if(!endF) return 1e9;
        return max(resF-resP, -1);
    }
};

/*
corner case hard to handle:
people can escape even if the fire spreads to the safehouse in the same minute
*/
```