# Min Cost Climbing Stairs

#: 746
Difficult: Easy
Tags: Array, dp-TimeSeqFromLastOne-Power
link: https://leetcode.com/problems/min-cost-climbing-stairs
Priority: Medium
Status: More Solution
Created time: June 27, 2023 9:52 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

You are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return *the minimum cost to reach the top of the floor*.

**Example 1:**

```
Input: cost = [10,15,20]
Output: 15
Explanation: You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.

```

**Example 2:**

```
Input: cost = [1,100,1,1,1,100,1,1,100,1]
Output: 6
Explanation: You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top.
The total cost is 6.

```

**Constraints:**

- `2 <= cost.length <= 1000`
- `0 <= cost[i] <= 999`

Solution:

check if like rob neigh, have statemachine solution

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n=cost.size();
        vector<int> dp(n+1,INT_MAX);
        dp[0]=0; dp[1]=0;
        for(int i=2; i<n+1; i++){
            dp[i]=min(dp[i-1]+cost[i-1], dp[i-2]+cost[i-2]);
        }
        return dp[n];
    }
};

/*
f(i): on stair i, min cost

init
    f(0)=0 / f(1)=0

f(i) = min(f(i-1)+cost[i-1] ,f(i-2)+cost[i-2])

return 
    f(n)
*/
```