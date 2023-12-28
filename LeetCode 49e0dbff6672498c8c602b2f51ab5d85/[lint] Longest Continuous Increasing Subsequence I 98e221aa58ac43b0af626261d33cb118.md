# [lint] Longest Continuous Increasing Subsequence II

#: 398
Difficult: Hard
Tags: DFS, DP, Matrix
link: https://www.lintcode.com/problem/longest-continuous-increasing-subsequence-ii/description
Priority: Low
Created time: September 10, 2022 10:23 AM
Last edited time: October 29, 2023 10:51 PM
practicedTimes: 1
source: jiuzhang, 算高DP上

Given an integer matrix. Find the longest increasing continuous subsequence in this matrix and return the length of it.

The longest increasing continuous subsequence here can start at any position and go up/down/left/right.

Example:
**Example 1:**

```
Input:
   [
     [1, 2, 3, 4, 5],
     [16,17,24,23,6],
     [15,18,25,22,7],
     [14,19,20,21,8],
     [13,12,11,10,9]
   ]
Output: 25
Explanation: 1 -> 2 -> 3 -> 4 -> 5 -> ... -> 25 (Spiral from outside to inside.)

```

**Example 2:**

```
Input:
   [
     [1, 2],
     [5, 3]
   ]
Output: 4
Explanation: 1 -> 2 -> 3 -> 5

```

Challenge
Assume that it is a N x M matrix. Solve this problem in O(NM) time and memory.

question <- subquestion, then DP, dfs to find subquestion solution, memorization

```cpp
class Solution {
public:
    /*
     * @param A: An integer matrix
     * @return: an integer
     */
    vector<int> dx{0,1,0,-1};
    vector<int> dy{1,0,-1,0};

    int longestIncreasingContinuousSubsequenceII(vector<vector<int>> &A) {
        //dp, dfs
        int m=A.size(); if(m==0) return 0;
        int n=A[0].size(); if(n==0) return 0;

        int res=0;
        vector<vector<int>> dp(m,vector<int>(n,-1));

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                res=max(res,dfs(A,dp,i,j));
            }
        }
        return res;
    }
    int dfs(vector<vector<int>> &A,vector<vector<int>> &dp,int x, int y){
        int m=A.size(), n=A[0].size();
        //invalid
        if(x<0 || x>=m || y<0 || y>=n) return 0;
        //in dp
        if(dp[x][y]!=-1){return dp[x][y];}
        //calculate dp and return with four directions
        dp[x][y]=1;//init
        for(int i=0;i<4;i++){
            int nx=x+dx[i];
            int ny=y+dy[i];
            if(nx<0 || nx>=m || ny<0 || ny>=n) continue;
            if(A[x][y]>A[nx][ny]){
                dp[x][y]=max(dp[x][y] , dfs(A,dp,nx,ny)+1);
            }
        }

        return dp[x][y];
    }
};
```