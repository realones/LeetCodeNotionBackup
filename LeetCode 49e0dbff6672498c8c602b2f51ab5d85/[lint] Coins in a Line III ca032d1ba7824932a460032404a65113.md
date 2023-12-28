# [lint] Coins in a Line III

#: 396
Difficult: Hard
Tags: DP, Game, dp-Pyr
link: https://www.lintcode.com/problem/coins-in-a-line-iii/description
Priority: Medium
Created time: September 10, 2022 10:51 AM
Last edited time: October 30, 2023 12:23 AM
practicedTimes: 1
source: jiuzhang, 算高DP上
related: Predict the Winner (Predict%20the%20Winner%20439491b47f7e4938af083811767798d8.md)

There are `n` coins in a line, and value of `i-th` coin is `values[i]`.

Two players take turns to take a coin from one of the ends of the line until there are no more coins left. The player with the larger amount of money wins.

Could you please decide **the first player** will win or lose?

Example:
**Example 1:**

```
Input: [3, 2, 2]
Output: true
Explanation: The first player takes 3 at first. Then they both take 2.

```

**Example 2:**

```
Input: [1, 20, 4]
Output: false
Explanation: The second player will take 20 whether the first player take 1 or 4.

```

Challenge
O(1) memory and O(n) time when `n` is even.

recursive:

```cpp
f(i,j) = 
    max of:
        sum(i,j) - f(i+1,j)
        sum(i,j) - f(i,j-1)
```

iterative:

搭建金字塔，从底层往上层

```cpp
for(int len=2; len<=n; len++){//length of a brick
    for(int i=0; i<n-len+1; i++){ //for each bricks
        int l=i, r=i+len-1;
        dp[l][r] = max(
            prefixSum(l,r) - dp[l+1][r  ],
            prefixSum(l,r) - dp[l  ][r-1])
    }
}
```

一维DP

```cpp
for(int len=2; len<=n; len++){//length of a brick
    for(int i=0; i<n-len+1; i++){ //for each bricks
        int l=i, r=i+len-1;
        dp[l] = max(prefixSum(l,r) - dp[l + 1],
                    prefixSum(l,r) - dp[l    ]);
    }
}

prefixSum(int l, int r){
    prefixSum[r] - (l==0?0:prefixSum[l-1]);
}
```

pyr1

```cpp
class Solution {
public:
    /**
     * @param values: a vector of integers
     * @return: a boolean which equals to true if the first player will win
     */
    bool firstWillWin(vector<int> &values) {
        // write your code here
        // pyramid
        // f(0,n-1) = max( sum(0,n-1)-f(1,n-1), sum(0,n-1)-f(0,n-2) )
        // return f(0,n-1)
        // init f(k,k) = values[k]
        // build prefix sum ary
        int n=values.size(); if(n==0) {return false;}
        vector<int> prefixSum(values);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        vector<vector<int>> f(n,vector<int>(n,0));
        for(int i=0; i<n; i++){
            f[i][i]=values[i];
        }
        for(int len=2;len<=n;len++){
            for(int l=0,r;r=l+len-1,r<n;l++){
                f[l][r]=max(
                            getPrefixSum(prefixSum,l,r)-f[l+1][r],
                            getPrefixSum(prefixSum,l,r)-f[l][r-1]);
            }
        }
        return f[0][n-1]*2>prefixSum[n-1];
    }

    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```

pry2

```cpp
class Solution {
public:
    /**
     * @param values: a vector of integers
     * @return: a boolean which equals to true if the first player will win
     */
    bool firstWillWin(vector<int> &values) {
        // write your code here
        // pyramid
        // f(0,n-1) = max( sum(0,n-1)-f(1,n-1), sum(0,n-1)-f(0,n-2) )
        // return f(0,n-1)
        // init f(k,k) = values[k]
        // build prefix sum ary
        int n=values.size(); if(n==0) {return false;}
        vector<int> prefixSum(values);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }
        vector<int> f(n,0);
        for(int i=0; i<n; i++){
            f[i]=values[i];
        }
        for(int len=2;len<=n;len++){
            for(int l=0,r;r=l+len-1,r<n;l++){
                f[l]=max(
                            getPrefixSum(prefixSum,l,r)-f[l],
                            getPrefixSum(prefixSum,l,r)-f[l+1]);
            }
        }
        return f[0]*2>prefixSum[n-1];
    }

    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```