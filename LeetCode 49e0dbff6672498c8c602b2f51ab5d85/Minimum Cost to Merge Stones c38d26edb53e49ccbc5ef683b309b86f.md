# Minimum Cost to Merge Stones

#: 1000
Difficult: Hard
Tags: dp-K_Ranges, dp-Pyr
link: https://leetcode.com/problems/minimum-cost-to-merge-stones/
Priority: High
Status: Dig More
Created time: December 20, 2022 9:56 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路

There are `n` piles of `stones` arranged in a row. The `ith` pile has `stones[i]` stones.

A move consists of merging exactly `k` **consecutive** piles into one pile, and the cost of this move is equal to the total number of stones in these `k` piles.

Return *the minimum cost to merge all piles of stones into one pile*. If it is impossible, return `-1`.

**Example 1:**

```
Input: stones = [3,2,4,1], k = 2
Output: 20
Explanation: We start with [3, 2, 4, 1].
We merge [3, 2] for a cost of 5, and we are left with [5, 4, 1].
We merge [4, 1] for a cost of 5, and we are left with [5, 5].
We merge [5, 5] for a cost of 10, and we are left with [10].
The total cost was 20, and this is the minimum possible.

```

**Example 2:**

```
Input: stones = [3,2,4,1], k = 3
Output: -1
Explanation: After any merge operation, there are 2 piles left, and we can't merge anymore.  So the task is impossible.

```

**Example 3:**

```
Input: stones = [3,5,1,2,6], k = 3
Output: 25
Explanation: We start with [3, 5, 1, 2, 6].
We merge [5, 1, 2] for a cost of 8, and we are left with [3, 8, 6].
We merge [3, 8, 6] for a cost of 17, and we are left with [17].
The total cost was 25, and this is the minimum possible.

```

**Constraints:**

- `n == stones.length`
- `1 <= n <= 30`
- `1 <= stones[i] <= 100`
- `2 <= k <= 30`

```cpp
class Solution {
public:
    int mergeStones(vector<int>& stones, int K) 
    {
        int N = stones.size();
        if ((N-1) % (K-1)) return -1;
        
        auto dp = vector<vector<vector<int>>>(N,vector<vector<int>>(N,vector<int>(K+1,INT_MAX)));
        
        vector<int>sum(N+1,0);
        for (int i=0; i<N; i++)
            sum[i+1] = sum[i]+stones[i];
            
        for (int i=0; i<N; i++) dp[i][i][1] = 0;        
        
        for (int len=2; len<=N; len++)
            for (int i=0; i<=N-len; i++)
            {
                int j = i+len-1;
                for (int k=2; k<=K; k++)
                {
                    if (k>len) continue;
                    for (int m=i; m<j; m++)
                    {
                        if (dp[i][m][1]==INT_MAX || dp[m+1][j][k-1]==INT_MAX) continue;   
                        dp[i][j][k] = min(dp[i][j][k], dp[i][m][1] + dp[m+1][j][k-1]);
                    }
                }
                if (dp[i][j][K]!=INT_MAX)
                    dp[i][j][1] = dp[i][j][K] + sum[j+1]-sum[i];
            }
        
        if (dp[0][N-1][1]==INT_MAX) return -1;
        else return dp[0][N-1][1];
    }
};

/*
dp solution:
N
N-(k-1)
N-(k-1)*2
...
K
1

how to split N to K pieces
K个区间先确定一个，剩下的K-1个区间甩锅 -> recursive

e.g.: dp[0][n-1][1] = dp[0][m][1] + dp[m+1][n-1][K-1] + sum[0,n-1]

dp[i][j][k] = min(dp[i][m][1] + dp[m+1][j][k-1]) i<=m<j
dp[i][j][1] = dp[i][j][k] + sumij
*/

/*
old - deprecated
dpij: min cost for i..j, if not valid -> -1, or max 100*30

e.g. k=3
last run:
part1 part2 part3

dpij = i..x1 + x1+1...x2 + x2+1...j back tracking?
dpijk: range ij, for k parts, the min cost (k: 1...K)
dpijk = dpij'k-1 + f(j'+1..j)  (j': i..j)
      = dpij'k-1 + dpj'+1j1

init: dpii1=[i]
return dp[first,last]
*/

/*
search solution: - T(0) too big
len:30,K=2
possible solution: 29! too big
29
28
...
*/
```