# Toss Strange Coins

#: 1230
Difficult: Medium
Tags: Probability, dp-backpack
link: https://leetcode.com/problems/toss-strange-coins/
Priority: High
Created time: June 2, 2023 3:59 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You have some coins.  The `i`-th coin has a probability `prob[i]` of facing heads when tossed.

Return the probability that the number of coins facing heads equals `target` if you toss every coin exactly once.

**Example 1:**

```
Input: prob = [0.4], target = 1
Output: 0.40000

```

**Example 2:**

```
Input: prob = [0.5,0.5,0.5,0.5,0.5], target = 0
Output: 0.03125

```

**Constraints:**

- `1 <= prob.length <= 1000`
- `0 <= prob[i] <= 1`
- `0 <= target <= prob.length`
- Answers will be accepted as correct if they are within `10^-5` of the correct answer.

```cpp
class Solution {
public:
    double probabilityOfHeads(vector<double>& prob, int target) {
        int n=prob.size();
        vector<vector<double>> dp(n+1,vector<double>(target+1,0));
        dp[0][0]=1;
        for(int i=1;i<n+1;i++){
            for(int j=0;j<target+1;j++){
                double notHead=dp[i-1][j]*(1-prob[i-1]);
                double head=((j-1>=0)?dp[i-1][j-1]:0)*prob[i-1];
                dp[i][j]=head+notHead;
            }
        }
        return dp.back().back();
    }
};
```