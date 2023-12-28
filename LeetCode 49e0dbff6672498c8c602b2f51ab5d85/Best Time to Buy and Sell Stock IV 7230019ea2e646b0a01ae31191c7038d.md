# Best Time to Buy and Sell Stock IV

#: 188
Difficult: Hard
Tags: DP, Game, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/
Priority: High
Status: More Solution, No Idea At First
Created time: September 29, 2022 12:40 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: k = 2, prices = [2,4,1]
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.

```

**Example 2:**

```
Input: k = 2, prices = [3,2,6,5,0,3]
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.

```

**Constraints:**

- `1 <= k <= 100`
- `1 <= prices.length <= 1000`
- `0 <= prices[i] <= 1000`

Solution 1: dp-state machine

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        vector<int> buy(k+1, INT_MIN);//kth time of buy
        vector<int> sell(k+1, 0);//kth time of sell
        for(auto price: prices){
            for(int i=1; i<k+1; i++){
                buy[i] = max(buy[i], sell[i-1]-price);
                sell[i] = max(sell[i], buy[i]+price);
            }
        }
        return max(buy.back(), sell.back());
    }
};
```

Solution 1: game

```cpp
//solution from stock III
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        vector<int> buy(k+1, INT_MAX);//kth time of buy
        vector<int> sell(k+1, 0);//kth time of sell
        for(auto price: prices){
            for(int i=1; i<k+1; i++){
                buy[i] = min(buy[i], price-sell[i-1]);
                sell[i] = max(sell[i], price-buy[i]);
            }
        }
        return sell.back();
    }
};
```