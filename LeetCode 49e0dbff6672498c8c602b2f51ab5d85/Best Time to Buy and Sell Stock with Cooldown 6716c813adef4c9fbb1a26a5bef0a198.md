# Best Time to Buy and Sell Stock with Cooldown

#: 309
Difficult: Medium
Tags: DP, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown
Priority: High
Status: More Solution
Created time: September 29, 2022 12:42 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路
related: Best Time to Buy and Sell Stock (Best%20Time%20to%20Buy%20and%20Sell%20Stock%20fec2d505349445dea1d7e454cc9d0a5a.md)

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]

```

**Example 2:**

```
Input: prices = [1]
Output: 0

```

**Constraints:**

- `1 <= prices.length <= 5000`
- `0 <= prices[i] <= 1000`

Solution 0: state machine

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size(); if(n==0) {return 0;}
        
        int ready=0;
        int haveStock=-1000;
        int cooldown=-1000;
        
        for(auto p: prices){
            int nxt_ready=max(ready,cooldown);
            int nxt_haveStock=max(haveStock,ready-p);
            int nxt_cooldown=haveStock+p;
            
            ready=nxt_ready;
            haveStock=nxt_haveStock;
            cooldown=nxt_cooldown;
        }
        
        return max({ready,haveStock,cooldown});
    }
};
```

Solution 1:

Let us define a ***state machine*** to model our agent. The state machine consists of three states, which we define as follows:

- state `held`: in this state, the agent holds a stock that it bought at some point before.
- state `sold`: in this state, the agent has just sold a stock right before entering this state. And the agent holds no stock at hand.
- state `reset`: first of all, one can consider this state as the starting point, where the agent holds no stock and did not sell a stock before. More importantly, it is also the *transient* state before the `held` and `sold`. Due to the ***cooldown*** rule, after the `sold` state, the agent can not immediately acquire any stock, but is *forced* into the `reset` state. One can consider this state as a "reset" button for the cycles of buy and sell transactions.

At any moment, the agent can only be in ***one*** state. The agent would transition to another state by performing some actions, namely:

- action `sell`: the agent sells a stock at the current moment. After this action, the agent would transition to the `sold` state.
- action `buy`: the agent acquires a stock at the current moment. After this action, the agent would transition to the `held` state.
- action `rest`: this is the action that the agent does no transaction, neither buy or sell. For instance, while holding a stock at the `held` state, the agent might simply do nothing, and at the next moment the agent would remain in the `held` state.

![Untitled](Best%20Time%20to%20Buy%20and%20Sell%20Stock%20with%20Cooldown%206716c813adef4c9fbb1a26a5bef0a198/Untitled.png)

sold[i]=hold[i−1]+price[i]
held[i]=max(held[i−1],reset[i−1]−price[i])
reset[i]=max(reset[i−1],sold[i−1])

```cpp
/*
state:
    held: have bought stock
    no-held:
        cooldown
        readyToBuy (init)
action:
    buy
    sell
    pass
    
*/
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int readyToBuy=0, held=INT_MIN, cooldown=0;
        
        //cooldown=held+sell
        //held=readyToBuy+buy or held+pass
        //readyToBuy=readyToBuy+pass or cooldown+pass
        
        for(auto price: prices){
            int preCooldown=cooldown;
            
            cooldown=held+price;
            held=max(readyToBuy-price, held);
            readyToBuy=max(readyToBuy, preCooldown);
        }
        
        return max(cooldown, readyToBuy);
    }
};

```

Solution1 with improvement:

```cpp
/*
buy[i] means before day i what is the maxProfit for any sequence end with buy.
sell[i] means before day i what is the maxProfit for any sequence end with sell.
rest[i] means before day i what is the maxProfit for any sequence end with rest.

A:
buy[i]  = max(rest[i-1]-price, buy[i-1])  //(1) We have to `rest` before we `buy` and 
sell[i] = max(buy[i-1]+price, sell[i-1])  //(2) we have to `buy` before we `sell`
rest[i] = max(sell[i-1], buy[i-1], rest[i-1])

One tricky point is how do you make sure you sell before you buy, since from the equations it seems that [buy, rest, buy] is entirely possible.

Well, the answer lies within the fact that buy[i] <= rest[i] which means rest[i] = max(sell[i-1], rest[i-1]). That made sure [buy, rest, buy] is never occurred.

A further observation is that and rest[i] <= sell[i] is also true therefore
rest[i] = sell[i-1]

so !!!!buy<=rest<=sell
=>

buy[i] = max(sell[i-2]-price, buy[i-1])
sell[i] = max(buy[i-1]+price, sell[i-1])

init:
buy[0]=-A[0],buy[1]=max(-A[0],-A[1])
sell[0]=0, sell[1]=A[1]-A[0]

return: sell[n-1]
*/

class Solution {
public:
    int maxProfit(vector<int> &prices) {
        int n=prices.size(); if(n<=1) return 0;
        vector<int> buy(2,0);//rolling array
        vector<int> sell(2,0);
        
        buy[0%2]=-prices[0]; buy[1%2]=max(-prices[0],-prices[1]);
        sell[0%2]=0; sell[1%2]=max(prices[1]-prices[0],0);
            
        for(int i=2;i<n;i++){
            buy[i%2] = max(sell[(i-2)%2]-prices[i], buy[(i-1)%2]);
            sell[i%2] = max(buy[(i-1)%2]+prices[i], sell[(i-1)%2]);
        }
        
        return sell[(n-1)%2];
    }
};
```

Solution 2: todo