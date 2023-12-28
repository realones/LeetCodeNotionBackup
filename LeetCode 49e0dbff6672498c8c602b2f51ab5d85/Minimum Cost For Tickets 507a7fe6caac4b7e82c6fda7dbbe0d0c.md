# Minimum Cost For Tickets

#: 983
Difficult: Medium
Tags: dp-TimeSeqFromLastOne-Power
link: https://leetcode.com/problems/minimum-cost-for-tickets/
Priority: Medium
Created time: January 27, 2023 6:27 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

You have planned some train traveling one year in advance. The days of the year in which you will travel are given as an integer array `days`. Each day is an integer from `1` to `365`.

Train tickets are sold in **three different ways**:

- a **1-day** pass is sold for `costs[0]` dollars,
- a **7-day** pass is sold for `costs[1]` dollars, and
- a **30-day** pass is sold for `costs[2]` dollars.

The passes allow that many days of consecutive travel.

- For example, if we get a **7-day** pass on day `2`, then we can travel for `7` days: `2`, `3`, `4`, `5`, `6`, `7`, and `8`.

Return *the minimum number of dollars you need to travel every day in the given list of days*.

**Example 1:**

```
Input: days = [1,4,6,7,8,20], costs = [2,7,15]
Output: 11
Explanation: For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 1-day pass for costs[0] = $2, which covered day 1.
On day 3, you bought a 7-day pass for costs[1] = $7, which covered days 3, 4, ..., 9.
On day 20, you bought a 1-day pass for costs[0] = $2, which covered day 20.
In total, you spent $11 and covered all the days of your travel.

```

**Example 2:**

```
Input: days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
Output: 17
Explanation: For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 30-day pass for costs[2] = $15 which covered days 1, 2, ..., 30.
On day 31, you bought a 1-day pass for costs[0] = $2 which covered day 31.
In total, you spent $17 and covered all the days of your travel.

```

**Constraints:**

- `1 <= days.length <= 365`
- `1 <= days[i] <= 365`
- `days` is in strictly increasing order.
- `costs.length == 3`
- `1 <= costs[i] <= 1000`

Solution:

```jsx
/*

*/
class Solution {
public:
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        int n=days.size(); if(n==0) return 0;
        
        vector<int> dp(n,INT_MAX); //dp i: min cost for days 0..i
        for(int i=0; i<n; i++){
            //1-day pass
            int j;
            for(j=i;j>=0 && (days[i]-days[j]<1);j--);
            int s1 = ((j>=0)?dp[j]:0)+costs[0];
            //7-day pass
            for(j=i;j>=0 && (days[i]-days[j]<7);j--);
            int s2 = ((j>=0)?dp[j]:0)+costs[1];
            //30-day pass
            for(j=i;j>=0 && (days[i]-days[j]<30);j--);
            int s3 = ((j>=0)?dp[j]:0)+costs[2];
            
            dp[i]=min({s1,s2,s3});
        }
        
        return dp[n-1];
    }
};
```