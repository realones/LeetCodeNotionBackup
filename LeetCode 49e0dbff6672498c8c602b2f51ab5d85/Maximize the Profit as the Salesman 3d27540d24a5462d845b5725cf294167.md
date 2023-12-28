# Maximize the Profit as the Salesman

#: 2830
Difficult: Medium
Tags: Array, Interval, dp-TimeSeqFromLastOne-Power
link: https://leetcode.com/problems/maximize-the-profit-as-the-salesman/
Priority: Medium
Status: No Idea At First
Created time: August 19, 2023 9:36 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given an integer `n` representing the number of houses on a number line, numbered from `0` to `n - 1`.

Additionally, you are given a 2D integer array `offers` where `offers[i] = [starti, endi, goldi]`, indicating that `ith` buyer wants to buy all the houses from `starti` to `endi` for `goldi` amount of gold.

As a salesman, your goal is to **maximize** your earnings by strategically selecting and selling houses to buyers.

Return *the maximum amount of gold you can earn*.

**Note** that different buyers can't buy the same house, and some houses may remain unsold.

**Example 1:**

```
Input: n = 5, offers = [[0,0,1],[0,2,2],[1,3,2]]
Output: 3
Explanation: There are 5 houses numbered from 0 to 4 and there are 3 purchase offers.
We sell houses in the range [0,0] to 1st buyer for 1 gold and houses in the range [1,3] to 3rd buyer for 2 golds.
It can be proven that 3 is the maximum amount of gold we can achieve.

```

**Example 2:**

```
Input: n = 5, offers = [[0,0,1],[0,2,10],[1,3,2]]
Output: 10
Explanation: There are 5 houses numbered from 0 to 4 and there are 3 purchase offers.
We sell houses in the range [0,2] to 2nd buyer for 10 golds.
It can be proven that 10 is the maximum amount of gold we can achieve.

```

**Constraints:**

- `1 <= n <= 105`
- `1 <= offers.length <= 105`
- `offers[i].length == 3`
- `0 <= starti <= endi <= n - 1`
- `1 <= goldi <= 103`

comment: 似乎有新套路，再看看solutions

似乎i → i+1 更简单，参照lee215答案

```cpp
class Solution {
public:
    int maximizeTheProfit(int n, vector<vector<int>>& offers) {
        //record all offers end with house[i]
        vector<vector<vector<int>>> houseOffers(n);
        for(auto offer: offers){
            int e=offer[1];
            houseOffers[e].push_back(offer);
        }
        
        vector<int> dp(n,0);
        for(int i=0; i<n; i++){
            dp[i]=i-1<0?0:dp[i-1];
            for(auto offer: houseOffers[i]){
                int s=offer[0], e=offer[1], g=offer[2];
                dp[i]=max({
                           dp[i],
                           (s==0?0:dp[s-1])+g
                         });
            }
        }
        return dp.back();
    }
};

/*
n: number of houses, 0~n-1
offer: [start,end,gold] buyer want to buy houses [start,end] for gold money

no overlap, just buy some but not need to buy all
*/
```