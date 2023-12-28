# Number of Ways to Stay in the Same Place After Some Steps

#: 1269
Difficult: Hard
Tags: dp-TimeSeqFromLastOne-Power, dp-bfs-stepByStep
link: https://leetcode.com/problems/number-of-ways-to-stay-in-the-same-place-after-some-steps/description/
Priority: Medium
Created time: September 24, 2023 10:18 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Knight Dialer (Knight%20Dialer%20ad2dc741f775475aadc6ce3fa768f12f.md), Count Vowels Permutation (Count%20Vowels%20Permutation%20dda110cbbdd045dcb165eceb07f72d7b.md)

You have a pointer at index `0` in an array of size `arrLen`. At each step, you can move 1 position to the left, 1 position to the right in the array, or stay in the same place (The pointer should not be placed outside the array at any time).

Given two integers `steps` and `arrLen`, return the number of ways such that your pointer is still at index `0` after **exactly** `steps` steps. Since the answer may be too large, return it **modulo** `109 + 7`.

**Example 1:**

```
Input: steps = 3, arrLen = 2
Output: 4
Explanation:There are 4 differents ways to stay at index 0 after 3 steps.
Right, Left, Stay
Stay, Right, Left
Right, Stay, Left
Stay, Stay, Stay

```

**Example 2:**

```
Input: steps = 2, arrLen = 4
Output: 2
Explanation: There are 2 differents ways to stay at index 0 after 2 steps
Right, Left
Stay, Stay

```

**Example 3:**

```
Input: steps = 4, arrLen = 2
Output: 8

```

**Constraints:**

- `1 <= steps <= 500`
- `1 <= arrLen <= 1e6`

Solution dp-bfs:

improved by “j<=min(arrLen, i+1)”, otherwise MLE

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int numWays(int steps, int arrLen) {
        vector<vector<ll>> dp(2,vector<ll>(arrLen+2,0));
        dp[0][1]=1;
        for(int i=1;i<steps+1;i++){//i steps
            for(int j=1;j<=min(arrLen, i+1);j++){//idx j for dp, x=j-1 for arr
                dp[i%2][j]=(dp[(i-1)%2][j-1] + dp[(i-1)%2][j] + dp[(i-1)%2][j+1])%M;
            }
        }
        return dp[steps%2][1];
    }
};

/*
recur:
dp i k: on idx i and remain k steps, how many solutions to go to idx 0

dp i k = dp(i-1/ i/ i+1, k-1)

init: dp[0][0]=1, dp[other][0]=0, dp[outOfScope][any]=0 <corner case>

return: dp 0 steps

=================>

bfs dp - iter:
dp k,i: on k step, on idx i, how many solutions go to this idx i

dp k,i = dp(i-1/ i/ i+1, k-1)

init: dp[0][0]=1, dp[0][other]=invalid

return: dp steps 0

*/
```