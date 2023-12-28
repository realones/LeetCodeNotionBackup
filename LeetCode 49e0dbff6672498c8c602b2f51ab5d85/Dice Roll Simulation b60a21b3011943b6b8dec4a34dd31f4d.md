# Dice Roll Simulation

#: 1223
Difficult: Hard
Tags: dp-TimeSeqFromLastOne-Power, dp-状态转移不仅依赖前一项
link: https://leetcode.com/problems/dice-roll-simulation/description/
Priority: High
Status: Dig More, More Solution, No Idea At First
Created time: September 21, 2023 11:32 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Paint House II (Paint%20House%20II%20c294748d1c2041c0b47e62eed7bb979d.md)

A die simulator generates a random number from `1` to `6` for each roll. You introduced a constraint to the generator such that it cannot roll the number `i` more than `rollMax[i]` (**1-indexed**) consecutive times.

Given an array of integers `rollMax` and an integer `n`, return *the number of distinct sequences that can be obtained with exact* `n` *rolls*. Since the answer may be too large, return it **modulo** `109 + 7`.

Two sequences are considered different if at least one element differs from each other.

**Example 1:**

```
Input: n = 2, rollMax = [1,1,2,2,2,3]
Output: 34
Explanation: There will be 2 rolls of die, if there are no constraints on the die, there are 6 * 6 = 36 possible combinations. In this case, looking at rollMax array, the numbers 1 and 2 appear at most once consecutively, therefore sequences (1,1) and (2,2) cannot occur, so the final answer is 36-2 = 34.

```

**Example 2:**

```
Input: n = 2, rollMax = [1,1,1,1,1,1]
Output: 30

```

**Example 3:**

```
Input: n = 3, rollMax = [1,1,1,2,2,3]
Output: 181

```

**Constraints:**

- `1 <= n <= 5000`
- `rollMax.length == 6`
- `1 <= rollMax[i] <= 15`

comment: 

to check wisdompeak has 3d dp

看看别人的solution，似乎我的解法和别人都不太一样

Solution - wrong:

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int dieSimulator(int n, vector<int>& rollMax) {
        vector<vector<ll>> dp(n,vector<ll>(6,0));
        for(int d=0; d<6; d++) dp[0][d]=1;
        for(int i=1; i<n; i++){
            for(int d=0;d<6;d++){
                dp[i][d] = accumulate(dp[i-1].begin(), dp[i-1].end(),0LL) - ((i-rollMax[d]>=0)?dp[i-rollMax[d]][d]:0);
            }
        }
        return accumulate(dp.back().begin(), dp.back().end(),0LL) %M;
    }
};

/*
dice: 1~6
rollMax[i]: number i is constrainted to rull at most rollMax[i] consequtive times

roll n times,
return: number of distinct sequences 

====>
build sequence len=n, 1by1
each element is from 1~6

there are constraints on consequtive same number for (1~6)

================================
thought:
similar to backpack with pick at most k times, but sequence matters, kind of bfs dp, but need to dig more

so, dp[idx][number] should be all possible dp[idx][any] sum - the one make it not possible which is dp[idx-consecutiveNum][number]

*/
```

Solution - corrected by chatgpt

e.g.

rollMax = [2,2,2,2,2,2]

dp[5][2] = (sum of dp[4][x], x:[0~5]) - (sum of dp[2][y], y:[0~5]&&y!=2)

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int dieSimulator(int n, vector<int>& rollMax) {
        vector<vector<ll>> dp(n,vector<ll>(6,0));
        for(int d=0; d<6; d++) dp[0][d]=1;
        for(int i=1; i<n; i++){
            for(int d=0;d<6;d++){
                //ll to_substract=(i-rollMax[d]>=0)?dp[i-rollMax[d]][d]:0;
                ll to_substract=0LL;
                if(i-rollMax[d]-1==-1) to_substract=1;
                else if(i-rollMax[d]-1>=0){
                    for(int k=0; k<6; k++){
                        if(k==d) continue;
                        to_substract+=dp[i-rollMax[d]-1][k];
                    }
                }
                dp[i][d] = (accumulate(dp[i-1].begin(), dp[i-1].end(),0LL)%M - to_substract%M);
            }
        }
        return (accumulate(dp.back().begin(), dp.back().end(),0LL) %M + M) %M;
    }
};

/*
dice: 1~6
rollMax[i]: number i is constrainted to rull at most rollMax[i] consequtive times

roll n times,
return: number of distinct sequences 

====>
build sequence len=n, 1by1
each element is from 1~6

there are constraints on consequtive same number for (1~6)

================================
thought:
similar to backpack with pick at most k times, but sequence matters, kind of bfs dp, but need to dig more

so, dp[idx][number] should be all possible dp[idx][any] sum - the one make it not possible which is dp[idx-consecutiveNum][number]

*/
```