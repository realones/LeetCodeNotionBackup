# Stone Game II

#: 1140
Difficult: Medium
Tags: DP, Game
link: https://leetcode.com/problems/stone-game-ii/
Priority: High
Status: More Solution, toRevisit
Created time: May 25, 2023 10:08 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Alice and Bob continue their games with piles of stones.  There are a number of piles **arranged in a row**, and each pile has a positive integer number of stones `piles[i]`.  The objective of the game is to end with the most stones.

Alice and Bob take turns, with Alice starting first.  Initially, `M = 1`.

On each player's turn, that player can take **all the stones** in the **first** `X` remaining piles, where `1 <= X <= 2M`.  Then, we set `M = max(M, X)`.

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

**Example 1:**

```
Input: piles = [2,7,9,4,4]
Output: 10
Explanation:  If Alice takes one pile at the beginning, Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 piles in total. If Alice takes two piles at the beginning, then Bob can take all three piles left. In this case, Alice get 2 + 7 = 9 piles in total. So we return 10 since it's larger.

```

**Example 2:**

```
Input: piles = [1,2,3,4,5,100]
Output: 104

```

**Constraints:**

- `1 <= piles.length <= 100`
- `1 <= piles[i] <= 104`

comment, check iterative solution

Solution: top-down

```cpp
class Solution {
public:
    int stoneGameII(vector<int>& piles) {
        int n=piles.size();
        vector<int> prefixSum(piles);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }

        vector<vector<int>> dp(n,vector<int>(n*2,INT_MIN));
        function<int(int,int)> helper = [&](int i, int m){
            if(i>n-1) return 0;
            if(dp[i][m]!=INT_MIN) return dp[i][m];
            for(int x=1;x<=2*m;x++){
                dp[i][m]=max(dp[i][m], getPrefixSum(prefixSum,i,n-1) - helper(i+x, max(m,x)) ); 
            }
            return dp[i][m];
        };
        return helper(0,1);
    }

    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
}

/*
dp i,m: 
    [i..n-1] max stone, for given m
    
dp i,m:
    sum(i,n-1) - dp(i+x,nextM based on x) , 1<=x<=2m
        if i>n-1, return 0;
       
init:
    all invalid, since above corner case handled init
    
return:
    dp 0, 1
*/  

/*
thought 1:
    f(l~n-1) = sum(l~n-1) - f(l+x,n-1) ,x decided by curM
    x with decide newM in subquestion "f(l+x,n-1)"
    so subquestion to question is R -> L
    m decidsion is L -> R
    can't 1d dp, so put m as 2d in dp.
*/

/*
question: what type of DP is it? not backpack?
*/
```