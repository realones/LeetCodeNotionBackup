# Unique Binary Search Trees

#: 96
Difficult: Medium
Tags: BST, Math, Tree, dp-Pyr
link: https://leetcode.com/problems/unique-binary-search-trees/
Priority: Low
Created time: September 21, 2023 9:28 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an integer `n`, return *the number of structurally unique **BST'**s (binary search trees) which has exactly* `n` *nodes of unique values from* `1` *to* `n`.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
Input: n = 3
Output: 5

```

**Example 2:**

```
Input: n = 1
Output: 1

```

**Constraints:**

- `1 <= n <= 19`

Solution pyr:

```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<vector<int>> dp(n,vector<int>(n,0));
        for(int i=0; i<n; i++){
            dp[i][i]=1;
        }
        for(int len=2;len<=n;len++){//len of a brick
            for(int l=0,r;r=l+len-1,r<n;l++){//for each brick
                for(int k=l; k<=r; k++){
                    dp[l][r]+=
                            (k-1<l?1:dp[l][k-1])
                            *
                            (k+1>r?1:dp[k+1][r]);
                }
            }
        }
        return dp[0][n-1];
    }
};
```

Solution old:

```cpp
class Solution {
public:
    int numTrees(int n) {
        if(n<1) return 0;
        vector<vector<int>> dp(n+1,vector<int>(n+1,-1));
        return helper(1,n,dp);        
    }
    //recur with memorization
    int helper(int start, int end, vector<vector<int>>& dp){
        if(start>=end) return 1;
        if(dp[start][end]!=-1) return dp[start][end];
        int res=0;
        for(int k=start;k<=end;k++){
            res+=helper(start,k-1,dp)*helper(k+1,end,dp);
        }
        
        dp[start][end]=res;
        return dp[start][end];
    }
};
```