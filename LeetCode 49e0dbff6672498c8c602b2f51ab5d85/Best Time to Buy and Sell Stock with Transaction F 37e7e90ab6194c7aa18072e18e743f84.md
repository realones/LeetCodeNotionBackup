# Best Time to Buy and Sell Stock with Transaction Fee

#: 714
Difficult: Medium
Tags: Array, DP, Greedy, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/
Priority: High
Status: No Idea At First
Created time: June 21, 2023 6:16 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75, leetcode_daily

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.

```

**Example 2:**

```
Input: prices = [1,3,7,5,10,3], fee = 3
Output: 6

```

**Constraints:**

- `1 <= prices.length <= 5 * 104`
- `1 <= prices[i] < 5 * 104`
- `0 <= fee < 5 * 104`

Solution[Incorrect] - Greedy - One very long input TestCase failure, not sure why:

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/solutions/3667442/c-greedy-solution-merge-intervals/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/solutions/3667442/c-greedy-solution-merge-intervals/)

```cpp
using ii=pair<int,int>;
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n=prices.size();
        if(n<=1) return 0;
        vector<ii> intervals;
        //get intervals
        for(int l=0,r=0;r<n&&r<n;l=r+1,r=l){
            while (r+1<n && prices[r+1]>prices[r]) r++;
            if(prices[r]>prices[l]) intervals.push_back({prices[l],prices[r]}); 
        }
        
        //merge intervals
        for(int i=0;i<intervals.size()-1;){//check with next
            auto& [curS, curE] = intervals[i];
            auto& [nxtS, nxtE] = intervals[i+1];
            int curGain=curE-curS-fee;
            int nxtGain=nxtE-nxtS-fee;
            int totalGain=nxtE-curS-fee;
            if(totalGain > curGain + nxtGain){
                curE=nxtE;
                intervals.erase(intervals.begin()+i+1);
            }
            else i++;
        }
        //calcu res
        int res=0;
        for(auto [l,r]: intervals){
            if(r-l>fee) res+=r-l-fee;
        }
        return res;
    }
};

/*
1 3 5 - once better than twice

if keep inc, find l , r, if gain-fee>0, trans

goal is to find each inc substr

if two substr A B,
A        B      fee=3
[1 3 7] [5 10] 3

7-5=2<3, still can contribute
merge intervals

*
```

Solution dp:

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int hold=INT_MIN/2, sold=0;
        for(int i=0; i<prices.size(); i++){
            int nHold= max(hold, sold-prices[i]);
            int nSold= max(sold, hold+prices[i]-fee);
            hold=nHold, sold=nSold;
        }
        return max(hold,sold);
    }
};

/*
status:
 hold
 sold
*/
```