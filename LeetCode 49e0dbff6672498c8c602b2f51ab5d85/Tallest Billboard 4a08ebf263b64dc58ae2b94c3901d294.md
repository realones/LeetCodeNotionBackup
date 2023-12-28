# Tallest Billboard

#: 956
Difficult: Hard
Tags: dp-backpack
link: https://leetcode.com/problems/tallest-billboard/
Priority: High
Status: Dig More
Created time: January 9, 2023 10:54 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路
related: Painting the Walls (Painting%20the%20Walls%204b6e131cda2142f28962adfa49d05bed.md)

You are installing a billboard and want it to have the largest height. The billboard will have two steel supports, one on each side. Each steel support must be an equal height.

You are given a collection of `rods` that can be welded together. For example, if you have rods of lengths `1`, `2`, and `3`, you can weld them together to make a support of length `6`.

Return *the largest possible height of your billboard installation*. If you cannot support the billboard, return `0`.

**Example 1:**

```
Input: rods = [1,2,3,6]
Output: 6
Explanation: We have two disjoint subsets {1,2,3} and {6}, which have the same sum = 6.

```

**Example 2:**

```
Input: rods = [1,2,3,4,5,6]
Output: 10
Explanation: We have two disjoint subsets {2,3,5} and {4,6}, which have the same sum = 10.

```

**Example 3:**

```
Input: rods = [1,2]
Output: 0
Explanation: The billboard cannot be supported, so we return 0.

```

**Constraints:**

- `1 <= rods.length <= 20`
- `1 <= rods[i] <= 1000`
- `sum(rods[i]) <= 5000`

**Solution**: 

combination sum cnt - one sum

```cpp
dp[i][j]: first i items, sum to j
vector<vector<bool>> dp(n+1,vector<bool>(maxSum+1,false));
dp[0][0]=true
for(int i=1;i<n+1;i++){
    for(int j=0;j<maxSum+1;j++){
        bool notTake=dp[i-1][j];
        bool take=j-cur>=0 ? dp[i-1][j-cur] : false;
        dp[i][j]=notTake||take;
    }
}
return dp.back().max
```

combination sum cnt - left sum and right sum

```cpp
dp[i][j][k]: 
    first i items, left sum = i, right sum = j
init:
    dp000 = true, other = false
dpijk =
    any of {
        notTake: dp[i-1][j][k]
        take to left: j-cur>=0 ? dp[i-1][j-cur][k] : false
        take to right: k-cur>=0 ? dp[i-1][j][k-cur] : false
    }
return:
    dp.back().max
```

dedup: for same diff, only maintain highest one

```cpp
dedup: for same diff, only maintain the highest one

dp[i][diff]:
    first i items, left-right=diff, max left sum
init:
    dp00 = 0, others = INT_MIN 
dp[i][diff] =
    max of
        notTake: dp[i-1][diff]
        take to left: diff-cur in range ? dp[i-1][diff-cur]+cur : INT_MIN;
        take to right: dif+cur in range ? dp[i-1][diff+cur] : INT_MIN;
return:
    dp.back()[0]
```

```jsx
/*
dp rod diff = left hegith
dp rod diff = max of
 1. not use: dp rod diff
 2. dp rod-1 |diff+/-rods[i]| + rods[i]
init: dp 0,0=0, other is invalid:INT_MIN
return dp n,0
*/

class Solution {
public:
    int B=5000;
    int tallestBillboard(vector<int>& rods) {
        int n=rods.size(); if(n==0) {return 0;}
        rods.insert(rods.begin(),0);
        
        vector<vector<int>> dp(n+1,vector<int>(B*2+1,INT_MIN));
        dp[0][0+B]=0;
        
        for(int i=1;i<n+1;i++){
            for(int d=-B;d<=B;d++){
                dp[i][d+B]=max({
                    dp[i-1][d+B],//not use
                    ((-B<=d-rods[i] && d-rods[i]<=B) ? dp[i-1][d-rods[i]+B] : INT_MIN),// add to right
                    ((-B<=d+rods[i] && d+rods[i]<=B) ? dp[i-1][d+rods[i]+B] + rods[i]: INT_MIN),// add to left
                });
            }
        }
        
        return dp[n][0+B];
    }
};
```