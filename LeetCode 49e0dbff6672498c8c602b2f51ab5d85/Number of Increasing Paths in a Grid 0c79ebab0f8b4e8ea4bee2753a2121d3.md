# Number of Increasing Paths in a Grid

#: 2328
Difficult: Hard
Tags: BFS, DFS, DP, Graph, Matrix, TopSorting
link: https://leetcode.com/problems/number-of-increasing-paths-in-a-grid/description/
Priority: Medium
Status: More Solution
Created time: June 17, 2023 6:00 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given an `m x n` integer matrix `grid`, where you can move from a cell to any adjacent cell in all `4` directions.

Return *the number of **strictly** **increasing** paths in the grid such that you can start from **any** cell and end at **any** cell.* Since the answer may be very large, return it **modulo** `109 + 7`.

Two paths are considered different if they do not have exactly the same sequence of visited cells.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/05/10/griddrawio-4.png](https://assets.leetcode.com/uploads/2022/05/10/griddrawio-4.png)

```
Input: grid = [[1,1],[3,4]]
Output: 8
Explanation: The strictly increasing paths are:
- Paths with length 1: [1], [1], [3], [4].
- Paths with length 2: [1 -> 3], [1 -> 4], [3 -> 4].
- Paths with length 3: [1 -> 3 -> 4].
The total number of paths is 4 + 3 + 1 = 8.

```

**Example 2:**

```
Input: grid = [[1],[2]]
Output: 3
Explanation: The strictly increasing paths are:
- Paths with length 1: [1], [2].
- Paths with length 2: [1 -> 2].
The total number of paths is 2 + 1 = 3.

```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 1000`
- `1 <= m * n <= 105`
- `1 <= grid[i][j] <= 105`

```cpp
using ll=long long;
ll M=1e9+7;

vector<int> dx{0,1,0,-1};
vector<int> dy{1,0,-1,0};

class Solution {
public:
    int countPaths(vector<vector<int>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        vector<vector<ll>> dp(m,vector<ll>(n,-1));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                dfs(matrix,dp,i,j);
            }
        }
        ll res=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                res=(res+dp[i][j])%M;
            }
        }
        std::cout<< "dp" << " -- " << std::endl; for (const auto& row : dp) { for (const auto& elem : row) { cout << elem << " "; } cout << endl; } std::cout<<std::endl;
        return (res+M)%M;
    }

    int dfs(vector<vector<int>> &matrix,vector<vector<ll>> &dp,int i, int j){
        int m=matrix.size(), n=matrix[0].size();
        //invalid
        if(i<0 || i>=m || j<0 || j>=n) return 0;
        //in dp
        if(dp[i][j]!=-1){return dp[i][j];}
        //calculate dp and return with four directions
        dp[i][j]=1;
        for(int k=0;k<4;k++){
            int ni=i+dx[k];
            int nj=j+dy[k];
            if(i==1 && j==0) {
                std::cout << "dp[0][0]" << " -- " << dp[0][0] << std::endl;
                std::cout << "ni" << " -- " << ni << std::endl;
                std::cout << "nj" << " -- " << nj << std::endl;
            }
            if(ni<0 || ni>=m || nj<0 || nj>=n) continue;
            if(matrix[i][j]<=matrix[ni][nj]) continue;
            dp[i][j] += dfs(matrix,dp,ni,nj);
            dp[i][j]%=M;
        }
        return dp[i][j];
    }
};
```