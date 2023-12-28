# [lint] Stone Game

#: 476
Difficult: Medium
Tags: DP, dp-Pyr, interval dp
link: https://www.lintcode.com/problem/476/
Priority: Low
Created time: September 10, 2022 11:01 AM
Last edited time: October 12, 2023 2:56 PM
source: jiuzhang, 算高DP下

Description

There is a stone game.At the beginning of the game the player picks `n` piles of stones in a line.

The goal is to merge the stones in one pile observing the following rules:

1. At each step of the game,the player can merge two adjacent piles to a new pile.
2. The cost of each combination is the sum of the weights of the two piles of stones combined.

Please find out the minimum cost of merging.

Example

**Example 1:**

```
Input: [3, 4, 3]
Output: 17

```

**Example 2:**

```
Input: [4, 1, 1, 4]
Output: 18
Explanation:
  1. Merge second and third piles => [4, 2, 4], score = 2
  2. Merge the first two piles => [6, 4]，score = 8
  3. Merge the last two piles => [10], score = 18

```

- 死胡同:容易想到的一个思路从小往大，枚举第一次合并是在哪?
- 记忆化搜索的思路，从大到小，先考虑最后的0-n-1 合并的总花费
- State:
    
    • dp[i][j] 表示把第i到第j个石子合并到一起的最小花费
    
- Function:
    
    * 预处理sum[i,j] 表示i到j所有石子价值和
    
    * dp[i][j] = min(dp[i][k]+dp[k+1][j]+sum[i,j]) 对于所有k属于{i,j}
    
- Intialize:
    
    • for each i
    
    • dp[i][i] = 0 //not A[i]
    
- Answer:
    
    • dp[0][n-1]
    

Solution 1:

```jsx
class Solution {
public:
    /**
     * @param a: An integer array
     * @return: An integer
     */

    /*
    state: f(i,j): tmp res from i to j, the min cost
    func:  f(i,j)=min(f(i,k)+f(k+1,j)+sum(i,j)) <need prefix sum array here>
    init:  f(i,i)=0
    return:f(0,n-1)
    */
    int stoneGame(vector<int> &nums) {
        // write your code here
        int n=nums.size(); if(n==0) {return 0;}
        vector<vector<int>> dp(n,vector<int>(n,0));

        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }

        for(int sz=2;sz<=n;sz++){
            for(int l=0,r;r=l+sz-1,r<n;l++){
                int sumlr=getPrefixSum(prefixSum,l,r);
                int preCost=INT_MAX;
                for(int k=l;k<r;k++){
                    preCost=min(preCost, dp[l][k]+dp[k+1][r]);
                }
                dp[l][r] = preCost+sumlr;
            }
        }

        return dp[0][n-1];
    }

    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```

Solution 2:

```cpp
class Solution {
public:
    /*
     * @param A: An integer array
     * @return: An integer
     */
    
    /*
    区间类DP：
    1. 求一段区间的解max/min/count
    2. 转移方程通过区间更新
    3. 从大到小的更新
    */
    
    /*
    state: f(i,j): tmp res from i to j, the min cost
    func:  f(i,j)=min(f(i,k)+f(k+1,j)+sum(i,j)) <need prefix sum array here>
    init:  f(i,i)=0
    return:f(0,n-1)
    */
     
    int stoneGame(vector<int> &A) {
        if(A.size()<=1) return 0;
        
        int n=A.size();
        vector<int> preSum(n,0);
        int sum=0;
        for(int i=0;i<n;i++){
            sum+=A[i];
            preSum[i]=sum;
        }
        
        vector<vector<int>> dp(n,vector<int>(n,-1));
        for(int i=0;i<n;i++){dp[i][i]=0;}
        
        return helper(A,preSum,dp,0,n-1);
        
    }
    
    int helper(vector<int> &A, vector<int>& preSum, vector<vector<int>>& dp, int start, int end){
        if(start>=end) return 0;
        if(dp[start][end]!=-1) return dp[start][end];
        
        int res=INT_MAX;
        int sumij=preSum[end]-(start==0? 0 : preSum[start-1]);
        
        for(int k=start;k<=end-1;k++){
            int l=helper(A,preSum,dp,start,k);
            int r=helper(A,preSum,dp,k+1,end);
            int tmp=l+r+sumij;
            res=min(res,tmp);
        }
        
        dp[start][end]=res;
        return dp[start][end];
    }
    
};
```