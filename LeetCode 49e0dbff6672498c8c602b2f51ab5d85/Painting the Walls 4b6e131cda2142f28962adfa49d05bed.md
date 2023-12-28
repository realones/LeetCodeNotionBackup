# Painting the Walls

#: 2742
Difficult: Hard
Tags: dp-backpack
link: https://leetcode.com/problems/painting-the-walls/
Priority: High
Status: No Idea At First, toRevisit, toSummarize
Created time: June 17, 2023 9:52 PM
Last edited time: November 28, 2023 9:28 PM
source: LC_Contests
related: Tallest Billboard (Tallest%20Billboard%204a08ebf263b64dc58ae2b94c3901d294.md)

You are given two **0-indexed** integer arrays, `cost` and `time`, of size `n` representing the costs and the time taken to paint `n` different walls respectively. There are two painters available:

- A **paid painter** that paints the `ith` wall in `time[i]` units of time and takes `cost[i]` units of money.
- A **free painter** that paints **any** wall in `1` unit of time at a cost of `0`. But the free painter can only be used if the paid painter is already **occupied**.

Return *the minimum amount of money required to paint the* `n` *walls.*

**Example 1:**

```
Input: cost = [1,2,3,2], time = [1,2,3,2]
Output: 3
Explanation: The walls at index 0 and 1 will be painted by the paid painter, and it will take 3 units of time; meanwhile, the free painter will paint the walls at index 2 and 3, free of cost in 2 units of time. Thus, the total cost is 1 + 2 = 3.

```

**Example 2:**

```
Input: cost = [2,3,4,2], time = [1,1,1,1]
Output: 4
Explanation: The walls at index 0 and 3 will be painted by the paid painter, and it will take 2 units of time; meanwhile, the free painter will paint the walls at index 1 and 2, free of cost in 2 units of time. Thus, the total cost is 2 + 2 = 4.

```

**Constraints:**

- `1 <= cost.length <= 500`
- `cost.length == time.length`
- `1 <= cost[i] <= 106`
- `1 <= time[i] <= 500`

solution:

dp[i][diff]

```cpp
class Solution {
public:
    int paintWalls(vector<int>& cost, vector<int>& time) {
        int n=cost.size();
        // backpack dp:
        // dp[i][diff]: the min cost to complete first 1 tasks, with time diff of paid - free
        vector<unordered_map<int,int>> dp(n+1);//todo use vector with offset
        // init:
        int accTime=accumulate(time.begin(), time.end(), 0);
        for(int i=0; i<=n; i++){
            for(int d=-n;d<=n;d++){
                dp[i][d]=INT_MAX/2;
            }
        }
        dp[0][0]=0;

        for(int i=0;i<n;i++){//i->i+1
            int c=cost[i], t=time[i];
            for(int j=-n;j<=n;j++){
                // use free:
                dp[i+1][j-1] = min(dp[i+1][j-1], dp[i][j]);
                // use paid:
                int j2= min(j+t,n);
                dp[i+1][j2] = min(dp[i+1][j2], dp[i][j]+c);
            }
        }
        // return:
        //     min of dp.back()[any >=0]
        //         vector<vector<int>> dp(n+1,vector<int>(packSz+1,INT_MAX));
        int res=INT_MAX;
        for(int j=0;j<=n;j++){
            res=min(res, dp.back()[j]);
        }
        return res;
    }
};

/*
wall:
    paid: time , cost
    free: 1    , 0   , can only be used when paid occupied, free is better
return:
    min cost to paint all walls
================================
wall: 1~500
cost: 1~1e6
time: 1~500
================================
better to pick wall that lower "cost" and longer "time", so we can use free painter "time" times
paid worker total time used should >= free works total time, better == than >

backpack dp:
dp[i][diff]: the min cost to complete first 1 tasks, with time diff of paid - free
dp[i][diff] min= 
                use free: dp[i-1][diff+1]
                use paid: dp[i-1][diff-t]
use i->i+1 to save space, since when paidTime>n, we don't put into consideration
init:
    dp[0][0]=0, other INT_MAX
return:
    min of dp.back()[any >=0]
*/
```

Incorrect Solution: 
ASSUME free ones not using time

```cpp
class Solution {
public:
    int paintWalls(vector<int>& cost, vector<int>& time) {
        int n=cost.size();
        int totalTime=accumulate(time.begin(), time.end(), 0);
        int packSz=(totalTime+1)/2;//round up
        vector<vector<int>> dp(n+1,vector<int>(packSz+1,5e8));
        dp[0][0]=0;
        for(int i=0;i<n;i++){
            for(int j=0;j<packSz+1;j++){
                //not take
                dp[i+1][j]=min(dp[i+1][j], dp[i][j]);
                //take
                int nxtTime=min(packSz,j+time[i]);
                dp[i+1][nxtTime]=min(dp[i+1][nxtTime], dp[i][j]+cost[i]);
            }
        }
        return dp.back().back();
    }
};

/*
not related to this question:
ASSUME free ones not using time: find a combination sum of time >= (round up)totalTime/2, with min sum of cost
backpack

dp[i][j]: first i items, with total j time, minTotalCost

range
i: 0 1 2 ... n
j: 0 1 2 ... totalTime/2 with round up

dp[i][j] min=
    not take cur item: dp[i-1][j]
    take cur item: dp[i-1][j-curTime]

i -> i+1

dp[i][j] -> 
    not take cur item: dp[i+1][j] min= dp[i][j]
    take cur item: dp[i+1][j+curTime] min= dp[i][j]

return dp.back().back();
*/
```