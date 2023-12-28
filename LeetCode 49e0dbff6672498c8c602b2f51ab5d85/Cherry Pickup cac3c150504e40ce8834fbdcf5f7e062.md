# Cherry Pickup

#: 741
Difficult: Hard
Tags: DP, Matrix, matrix-diagTraverse
link: https://leetcode.com/problems/cherry-pickup/
Priority: High
Status: Dig More, More Solution, No Idea At First
Created time: June 28, 2023 12:52 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given an `n x n` `grid` representing a field of cherries, each cell is one of three possible integers.

- `0` means the cell is empty, so you can pass through,
- `1` means the cell contains a cherry that you can pick up and pass through, or
- `1` means the cell contains a thorn that blocks your way.

Return *the maximum number of cherries you can collect by following the rules below*:

- Starting at the position `(0, 0)` and reaching `(n - 1, n - 1)` by moving right or down through valid path cells (cells with value `0` or `1`).
- After reaching `(n - 1, n - 1)`, returning to `(0, 0)` by moving left or up through valid path cells.
- When passing through a path cell containing a cherry, you pick it up, and the cell becomes an empty cell `0`.
- If there is no valid path between `(0, 0)` and `(n - 1, n - 1)`, then no cherries can be collected.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/12/14/grid.jpg](https://assets.leetcode.com/uploads/2020/12/14/grid.jpg)

```
Input: grid = [[0,1,-1],[1,0,-1],[1,1,1]]
Output: 5
Explanation: The player started at (0, 0) and went down, down, right right to reach (2, 2).
4 cherries were picked up during this single trip, and the matrix becomes [[0,1,-1],[0,0,-1],[0,0,0]].
Then, the player went left, up, up, left to return home, picking up one more cherry.
The total number of cherries picked up is 5, and this is the maximum possible.

```

**Example 2:**

```
Input: grid = [[1,1,-1],[1,-1,1],[-1,1,1]]
Output: 0

```

**Constraints:**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 50`
- `grid[i][j]` is `1`, `0`, or `1`.
- `grid[0][0] != -1`
- `grid[n - 1][n - 1] != -1`

comment: for {i,j} and {x,y}, they should be same distant from topLeft so to check if they are same cell or not.

Solution dp * 2 and diagTraverse:

```cpp
class Solution {
public:
    using ii=pair<int,int>;
    
    int cherryPickup(vector<vector<int>>& matrix) {
        int m=matrix.size(); 
        int n=matrix[0].size();
        int dist=m+n-2;

        vector<vector<vector<vector<int>>>> dp(n,vector<vector<vector<int>>>(n,vector<vector<int>>(n,vector<int>(n,INT_MIN))));

        function f = [&](int i, int j, int x ,int y)->int{
            if(i<0 || j<0 || x<0 || y<0) return INT_MIN;
            if(matrix[i][j]==-1 || matrix[x][y]==-1) return INT_MIN;
            return dp[i][j][x][y];
        };

        for(int d=0; d<=dist; d++){
            for(int i=max(d-m+1,0),j; j=d-i,i<=min(d,m-1); i++){
                if(matrix[i][j]==-1) continue;
                for(int x=i,y; y=d-x,x<=min(d,m-1); x++){
                    if(matrix[x][y]==-1) continue;
                    if(i==0 && j==0 && x==0 && y==0) {dp[0][0][0][0]=matrix[i][j];continue;}
                    dp[i][j][x][y]=
                        max({
                            f(i-1,j,x-1,y),
                            f(i-1,j,x,y-1),
                            f(i,j-1,x-1,y),
                            f(i,j-1,x,y-1)
                        })
                        +
                        (i==x? matrix[i][j] : matrix[i][j]+matrix[x][y]);
                }
            }
        }
        return max(dp.back().back().back().back(),0);
    }
};
```

Solution wisdompeak, dp*2

todo: check special handling of dp sz+=1

```cpp
class Solution {
public:
    int cherryPickup(vector<vector<int>>& grid) 
    {
        int N = grid.size();
        int dp[N+1][N+1][N+1];
        
        for (int i=0; i<=N; i++)
            for (int j=0; j<=N; j++)
                for (int x=0; x<=N; x++)
                    dp[i][j][x] = INT_MIN;
        
        for (int i=1; i<=N; i++)
            for (int j=1; j<=N; j++)
                for (int x=1; x<=N; x++)
                {
                    int y = i+j-x;
                    if (y<1||y>N) continue;
                    if (grid[i-1][j-1]==-1||grid[x-1][y-1]==-1) continue;
                    if (i==1&&j==1&&x==1)
                    {
                        dp[i][j][x] = grid[0][0];
                        continue;
                    }

                    dp[i][j][x] = max(dp[i][j][x], dp[i-1][j][x-1]);
                    dp[i][j][x] = max(dp[i][j][x], dp[i][j-1][x-1]);
                    dp[i][j][x] = max(dp[i][j][x], dp[i-1][j][x]);
                    dp[i][j][x] = max(dp[i][j][x], dp[i][j-1][x]);
                    
                    if (i==x && j==y)
                        dp[i][j][x] += grid[i-1][j-1];
                    else
                        dp[i][j][x] += grid[i-1][j-1] + grid[x-1][y-1];
                    
                }
        
        return max(0,dp[N][N][N]);
    }
};
```

Solution Greedy - Wrong:

check editorial for details: [https://leetcode.com/problems/cherry-pickup/editorial/](https://leetcode.com/problems/cherry-pickup/editorial/)

failed test case:

11100
00101
10100
00100
00111

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int cherryPickup(vector<vector<int>>& matrix) {
        int m=matrix.size(), n=matrix[0].size();//check case ==0
        //step 1
        vector<vector<int>> dp(m,vector<int>(n,-1));
        vector<vector<ii>> parent(m,vector<ii>(n,{-1,-1}));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==-1) continue;//invalid
                int left=j-1>=0?dp[i][j-1]:0;
                int up=i-1>=0?dp[i-1][j]:0;
                if(left==-1 && up==-1) continue;//invalid
                //begin parent handling
                if(left>up) parent[i][j]={i,j-1};
                else if(left<up) parent[i][j]={i-1,j};
                else {//==
                    if(i-1>=0) parent[i][j]={i-1,j};
                    else if(j-1>=0) parent[i][j]={i,j-1};
                }
                //end parent handling
                dp[i][j]=max(left,up) + (matrix[i][j]==1);
            }
        }
        int step1=dp.back().back();
        if(step1<=0) return 0;
        //step1.5 take cherry
        ii cur{n-1,n-1};
        while(cur!=pair{-1,-1}){
            matrix[cur.first][cur.second]=0;
            cur=parent[cur.first][cur.second];
        }
        //step 2
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==-1) continue;//invalid
                int left=j-1>=0?dp[i][j-1]:0;
                int up=i-1>=0?dp[i-1][j]:0;
                if(left==-1 && up==-1) continue;//invalid
                dp[i][j]=max(left,up) + (matrix[i][j]==1);
            }
        }
        int step2=dp.back().back();
        return step1+step2;
    }
};
/*
0 empty
1 cherry
-1 wall

if no path, return 0
================================

dp[i][j] = 
            not wall max(dp l, dp u) + cur have cherry
            wall 0

parent[i][j]

*/
```