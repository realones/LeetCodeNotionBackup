# Best Time to Buy and Sell Stock III

#: 123
Difficult: Hard
Tags: DP, Game, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/
Priority: Medium
Created time: September 29, 2022 12:30 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150, wp动归套路

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

**Example 2:**

```
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.

```

**Example 3:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.

```

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 105`

Solution 0: state machine

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size(); if(n==0) {return 0;}
        
        int notBuyAny=0;
        int have1stStock=-1e5;
        int completed1stStock=-1e5;
        int have2ndStock=-1e5;
        int completed2ndStock=-1e5;
        
        for(int i=0; i<n; i++){
            int nxt_have1stStock=max(have1stStock, notBuyAny-prices[i]);
            int nxt_completed1stStock=max(completed1stStock, have1stStock+prices[i]);
            int nxt_have2ndStock=max(have2ndStock, completed1stStock-prices[i]);
            int nxt_completed2ndStock=max(completed2ndStock, have2ndStock+prices[i]);
            
            have1stStock=nxt_have1stStock;
            completed1stStock=nxt_completed1stStock;
            have2ndStock=nxt_have2ndStock;
            completed2ndStock=nxt_completed2ndStock;
        }
        
        return max({notBuyAny,have1stStock,completed1stStock,have2ndStock,completed2ndStock});
    }
};
```

Solution 1: dp

```cpp
/*
solution 1:
Divide and Conquer
f(0,n-1): result
f(0,n-1) = max(f(0,k)+f(k+1,n-1)), k:0...n-1 -> T: O(n*2)

to improve: Bidirectional DP
O(N) to get left profits: f(0,k), k:0...n-1
O(N) to get right profits: f(k,n-1), k:0...n-1
O(N) to calcu f(0,k) + f(k+1,n-1) k: 0...n-1
*/
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size(); if(n==0) {return 0;}
        int res=0;
        vector<int> leftProfits(n,0);
        vector<int> rightProfits(n,0);
        //calcu left and right profits
        int preMin=prices[0];
        for(int i=1; i<n; i++){
            leftProfits[i]=max(leftProfits[i-1], prices[i]-preMin);
            preMin=min(preMin, prices[i]);
        }
        
        int postMax=prices[n-1];
        for(int i=n-2; i>=0; i--){
            rightProfits[i]=max(rightProfits[i+1], postMax-prices[i]);
            postMax=max(postMax, prices[i]);
        }
        
        //twice
        for(int i=0; i<n-1; i++){
            res=max(res, leftProfits[i] + rightProfits[i+1]);
        }
        //once
        res=max(res,leftProfits[n-1]);
        
        return res;
    }
};
```

Solution 2: game

```cpp
//game dp
/*
t1_cost: min cost of buying the stock in transaction 1, fk=min(prices 0,1,..,k)
t1_sell: max profit of selling the stock in transaction 1, fk=max(f[k-1], pricesk-preMin)
*/
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy1=INT_MAX, buy2=INT_MAX;
        int sell1=0, sell2=0;
        
        for(auto p: prices){
            //1tran
            buy1=min(buy1,p);
            sell1=max(sell1, p-buy1);
            //2tran
            buy2=min(buy2,p-sell1);
            sell2=max(sell2, p-buy2);
        }
        return sell2;
    }
};
```