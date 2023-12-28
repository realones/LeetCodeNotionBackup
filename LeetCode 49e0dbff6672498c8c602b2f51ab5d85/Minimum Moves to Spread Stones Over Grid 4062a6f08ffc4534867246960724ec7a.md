# Minimum Moves to Spread Stones Over Grid

#: 2850
Difficult: Medium
Tags: BFS, BackTracking, DFS, DP, Matrix, recursion
link: https://leetcode.com/problems/minimum-moves-to-spread-stones-over-grid/description/
Priority: High
Status: More Solution, No Idea At First
Created time: December 15, 2023 11:01 PM
Last edited time: December 15, 2023 11:15 PM
source: microsoftFreq

You are given a **0-indexed** 2D integer matrix `grid` of size `3 * 3`, representing the number of stones in each cell. The grid contains exactly `9` stones, and there can be **multiple** stones in a single cell.

In one move, you can move a single stone from its current cell to any other cell if the two cells share a side.

Return *the **minimum number of moves** required to place one stone in each cell*.

**Example 1:**

![https://assets.leetcode.com/uploads/2023/08/23/example1-3.svg](https://assets.leetcode.com/uploads/2023/08/23/example1-3.svg)

```
Input: grid = [[1,1,0],[1,1,1],[1,2,1]]
Output: 3
Explanation: One possible sequence of moves to place one stone in each cell is:
1- Move one stone from cell (2,1) to cell (2,2).
2- Move one stone from cell (2,2) to cell (1,2).
3- Move one stone from cell (1,2) to cell (0,2).
In total, it takes 3 moves to place one stone in each cell of the grid.
It can be shown that 3 is the minimum number of moves required to place one stone in each cell.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2023/08/23/example2-2.svg](https://assets.leetcode.com/uploads/2023/08/23/example2-2.svg)

```
Input: grid = [[1,3,0],[1,0,0],[1,0,3]]
Output: 4
Explanation: One possible sequence of moves to place one stone in each cell is:
1- Move one stone from cell (0,1) to cell (0,2).
2- Move one stone from cell (0,1) to cell (1,1).
3- Move one stone from cell (2,2) to cell (1,2).
4- Move one stone from cell (2,2) to cell (2,1).
In total, it takes 4 moves to place one stone in each cell of the grid.
It can be shown that 4 is the minimum number of moves required to place one stone in each cell.

```

**Constraints:**

- `grid.length == grid[i].length == 3`
- `0 <= grid[i][j] <= 9`
- Sum of `grid` is equal to `9`.

Solution recursion(top-down)+ dp

```cpp
class Solution {
public:
    int minimumMoves(vector<vector<int>>& matrix) {
        unordered_map<string,int> dp;
        return helper(matrix,dp);
    }

    int helper(vector<vector<int>>& matrix, unordered_map<string,int>& dp){
        //end case
        string s="";
        for(int i=0;i<3;i++){
            for(int j=0;j<3;j++){
                s+=matrix[i][j]+'0';
            }
        }
        if(s=="111111111") return 0;

        //process
        if(dp.count(s)) return dp[s];
        int res=INT_MAX;
        //for each cell >1
        for(int i=0;i<3;i++){
            for(int j=0;j<3;j++){
                if(matrix[i][j]<=1) continue;
                //for each cell ==0
                for(int i0=0; i0<3; i0++){
                    for(int j0=0; j0<3; j0++){
                        if(matrix[i0][j0]!=0) continue;
                        //from >1 ==> 0
                        matrix[i][j]--;
                        matrix[i0][j0]++;
                        int curMove= abs(i-i0)+abs(j-j0);
                        int followingMove=helper(matrix,dp);
                        int move=curMove+followingMove;
                        res=min(res,move);
                        matrix[i0][j0]--;
                        matrix[i][j]++;
                    }
                }
            }
        }
        dp[s]=res;
        return res;
    }
};

/*
backtracking:

group of >1 cells
group of 0 cells

end when all cell ==1

for each group of >1 cell - 9
    for each 0 cells - 9
        give to 0 cells
        move+=hamilton dist
        next recur
        get it back
        move-=hamilton dist

with dp - memorization
*/
```

Solution: backtracking

```cpp
class Solution {
public:
    int minimumMoves(vector<vector<int>>& matrix) {
        int res=INT_MAX, move=0;
        backtracking(matrix,res,move);
        return res;
    }

    void backtracking(vector<vector<int>>& matrix, int& res, int& move){
        int cnt1=0;
        for(int i=0;i<3;i++){
            for(int j=0;j<3;j++){
                if(matrix[i][j]==1) cnt1++;
            }
        }
        if(cnt1==9) res=min(res,move);

        //for each cell >1
        for(int i=0;i<3;i++){
            for(int j=0;j<3;j++){
                if(matrix[i][j]<=1) continue;
                //for each cell ==0
                for(int i0=0; i0<3; i0++){
                    for(int j0=0; j0<3; j0++){
                        if(matrix[i0][j0]!=0) continue;
                        matrix[i][j]--;
                        matrix[i0][j0]++;
                        move+= abs(i-i0)+abs(j-j0);
                        backtracking(matrix,res,move);
                        move-= abs(i-i0)+abs(j-j0);
                        matrix[i0][j0]--;
                        matrix[i][j]++;
                    }
                }
            }
        }
    }
};

/*
backtracking:

group of >1 cells
group of 0 cells

end when all cell ==1

for each group of >1 cell - 9
    for each 0 cells - 9
        give to 0 cells
        move+=hamilton dist
        next recur
        get it back
        move-=hamilton dist

with dp - memorization
*/
```