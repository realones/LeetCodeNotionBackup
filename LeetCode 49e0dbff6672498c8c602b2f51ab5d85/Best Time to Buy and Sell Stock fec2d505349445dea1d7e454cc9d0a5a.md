# Best Time to Buy and Sell Stock

#: 121
Difficult: Easy
Tags: Array, Greedy, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock/
Priority: Pass
Status: More Solution
Created time: September 25, 2022 5:29 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150
related: Best Time to Buy and Sell Stock with Cooldown (Best%20Time%20to%20Buy%20and%20Sell%20Stock%20with%20Cooldown%206716c813adef4c9fbb1a26a5bef0a198.md)

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

```

**Example 2:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.

```

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res=0, preMin=INT_MAX;
        for(auto p: prices){
            res=max(res, p-preMin);
            preMin=min(preMin,p);
        }
        return res;
    }
};
```