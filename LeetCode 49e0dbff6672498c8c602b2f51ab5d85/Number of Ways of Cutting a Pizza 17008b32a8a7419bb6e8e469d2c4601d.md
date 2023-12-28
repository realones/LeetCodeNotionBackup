# Number of Ways of Cutting a Pizza

#: 1444
Difficult: Hard
Tags: DP, Matrix, Prefix
link: https://leetcode.com/problems/number-of-ways-of-cutting-a-pizza
Priority: High
Status: CalcuComplexity, HardToImplement, More Solution, No Idea At First, checkBetterSolution
Created time: June 28, 2023 12:35 AM
Last edited time: November 17, 2023 5:54 PM
practicedTimes: 1
source: HuaHua, ticktokFreq

Given a rectangular pizza represented as a `rows x cols` matrix containing the following characters: `'A'` (an apple) and `'.'` (empty cell) and given the integer `k`. You have to cut the pizza into `k` pieces using `k-1` cuts.

For each cut you choose the direction: vertical or horizontal, then you choose a cut position at the cell boundary and cut the pizza into two pieces. If you cut the pizza vertically, give the left part of the pizza to a person. If you cut the pizza horizontally, give the upper part of the pizza to a person. Give the last piece of pizza to the last person.

*Return the number of ways of cutting the pizza such that each piece contains **at least** one apple.* Since the answer can be a huge number, return this modulo 10^9 + 7.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/04/23/ways_to_cut_apple_1.png](https://assets.leetcode.com/uploads/2020/04/23/ways_to_cut_apple_1.png)

```
Input: pizza = ["A..","AAA","..."], k = 3
Output: 3
Explanation: The figure above shows the three ways to cut the pizza. Note that pieces must contain at least one apple.

```

**Example 2:**

```
Input: pizza = ["A..","AA.","..."], k = 3
Output: 1

```

**Example 3:**

```
Input: pizza = ["A..","A..","..."], k = 1
Output: 1

```

**Constraints:**

- `1 <= rows, cols <= 50`
- `rows == pizza.length`
- `cols == pizza[i].length`
- `1 <= k <= 10`
- `pizza` consists of characters `'A'` and `'.'` only.

comment: done by myself, from the Editorial, i see shorter solutions

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int m,n;
    vector<vector<int>> prefixSum;

    int ways(vector<string>& matrix, int K) {
        m=matrix.size(), n=matrix[0].size();
        buildPrefixSum(matrix);

        //dp(x,y,k):  for sub matrix from(x,y), cut into k pieces, num of ways of cut
        vector<vector<vector<ll>>> dp(m,vector<vector<ll>>(n,vector<ll>(K+1,-1)));
        //dfs dp
        function<ll(int,int,int)> f = [&](int x, int y, int k){
            if(dp[x][y][k]!=-1) return dp[x][y][k];
            //dfs end condition
            if(k==1) {dp[x][y][k]=valid(x,y,m-1,n-1); return dp[x][y][k];}
            ll res=0;
            //cut vertically
            for(int nx=x+1; nx<m; nx++){
                if(valid(x,y,nx-1,n-1)) res+=f(nx,y,k-1);
            }
            //cut horizontally
            for(int ny=y+1; ny<n; ny++){
                if(valid(x,y,m-1,ny-1)) res+=f(x,ny,k-1);
            }
            res%=M;
            dp[x][y][k]=res;
            return res;
        };
        return f(0,0,K);
    }

    void buildPrefixSum(vector<string>& matrix) {
        prefixSum = vector<vector<int>>(m,vector<int>(n,0));
        prefixSum[0][0]=matrix[0][0]=='A';
        for(int i=1; i<m; i++) prefixSum[i][0]=prefixSum[i-1][0] + (matrix[i][0]=='A');
        for(int j=1; j<n; j++) prefixSum[0][j]=prefixSum[0][j-1] + (matrix[0][j]=='A');
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                prefixSum[i][j]= prefixSum[i-1][j]+ prefixSum[i][j-1]- prefixSum[i-1][j-1] + (matrix[i][j]=='A');
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return prefixSum[row2][col2] 
                - ((col1-1>=0)?prefixSum[row2][col1-1]:0)
                - ((row1-1>=0)?prefixSum[row1-1][col2]:0)
                + ((row1-1>=0 && col1-1>=0)?prefixSum[row1-1][col1-1]:0);
    }

    bool valid(int row1, int col1, int row2, int col2) {
        return sumRegion(row1,col1,row2,col2)>0;
    }
};

/*
m,n: 1~50
k:   1~10
'A': apple, '.': empty
================================
each time cut:
    give the left
    give the upper
return: ways of cut to k pieces, each piece has >=1 apples.
================================

dp(x,y,k):  for sub matrix from(x,y), cut into k pieces, num of ways of cut

dp(x,y,k) = 
    cut vertically/horizontally
    dp(x',y', k-1)

init: dp(?,?,1) = 
        if >=1 apples, 1
        if ==0 apples, 0

return: dp(0,0,K)

T: 50*50*10*verifyIfBlockHasA_preprocessed=1

backTracking?

================================
subquestion: verifyIfBlockHasA
matrix prefix sum

*/
```