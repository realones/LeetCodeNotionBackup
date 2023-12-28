# Knight Probability in Chessboard

#: 688
Difficult: Medium
Tags: DP, Probability
link: https://leetcode.com/problems/knight-probability-in-chessboard/
Priority: Medium
Status: CalcuComplexity
Created time: July 21, 2023 8:46 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

On an `n x n` chessboard, a knight starts at the cell `(row, column)` and attempts to make exactly `k` moves. The rows and columns are **0-indexed**, so the top-left cell is `(0, 0)`, and the bottom-right cell is `(n - 1, n - 1)`.

A chess knight has eight possible moves it can make, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.

![https://assets.leetcode.com/uploads/2018/10/12/knight.png](https://assets.leetcode.com/uploads/2018/10/12/knight.png)

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.

The knight continues moving until it has made exactly `k` moves or has moved off the chessboard.

Return *the probability that the knight remains on the board after it has stopped moving*.

**Example 1:**

```
Input: n = 3, k = 2, row = 0, column = 0
Output: 0.06250
Explanation: There are two moves (to (1,2), (2,1)) that will keep the knight on the board.
From each of those positions, there are also two moves that will keep the knight on the board.
The total probability the knight stays on the board is 0.0625.

```

**Example 2:**

```
Input: n = 1, k = 0, row = 0, column = 0
Output: 1.00000

```

**Constraints:**

- `1 <= n <= 25`
- `0 <= k <= 100`
- `0 <= row, column <= n - 1`

todo: any other solutions?

Solution:

```cpp
class Solution {
public:
    double knightProbability(int n, int k, int row, int column) {
        vector<vector<vector<double>>> dp(n,vector<vector<double>>(n,vector<double>(k+1,-1)));
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                dp[i][j][0]=1;
            }
        }
        return helper(n,k,row,column,dp);
    }

    vector<int> dx={-2,-2,-1,-1,1,1,2,2};
    vector<int> dy={-1,1,-2,2,-2,2,-1,1};
    double helper(int n, int k, int x, int y, vector<vector<vector<double>>>& dp){
        if(x<0 || x>n-1 || y<0 || y>n-1) return 0;
        if(dp[x][y][k]!=-1) return dp[x][y][k];
        
        double res=0;
        for(int t=0; t<8; t++){
            int nx=x+dx[t];
            int ny=y+dy[t];
            res+=helper(n,k-1,nx,ny,dp);
        }
        res/=8;

        dp[x][y][k]=res;
        return dp[x][y][k];
    }
};

/*
{position, moveCnt} = alive rate
can't use visited

for each
    dp[any,0] = 1

dp[?,k] =
          (dp[nxtPosition1,k-1] if position out of board, 0
          dp[nxtPosition2,k-1] 
          dp[nxtPosition3,k-1] 
          dp[nxtPosition4,k-1] 
          dp[nxtPosition5,k-1] 
          dp[nxtPosition6,k-1] 
          dp[nxtPosition7,k-1] 
          dp[nxtPosition8,k-1])/8
        
return dp[row_col, k]

*/
```