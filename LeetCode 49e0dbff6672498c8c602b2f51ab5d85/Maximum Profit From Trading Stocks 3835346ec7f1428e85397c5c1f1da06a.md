# Maximum Profit From Trading Stocks

#: 2291
Difficult: Medium
Tags: DP, dp-backpack
link: https://leetcode.com/problems/maximum-profit-from-trading-stocks/
Priority: Medium
Status: More Solution
Created time: September 29, 2022 12:16 PM
Last edited time: October 12, 2023 2:56 PM

You are given two **0-indexed** integer arrays of the same length `present` and `future` where `present[i]` is the current price of the `ith` stock and `future[i]` is the price of the `ith` stock a year in the future. You may buy each stock at most **once**. You are also given an integer `budget` representing the amount of money you currently have.

Return *the maximum amount of profit you can make.*

**Example 1:**

```
Input: present = [5,4,6,2,3], future = [8,5,4,3,5], budget = 10
Output: 6
Explanation: One possible way to maximize your profit is to:
Buy the 0th, 3rd, and 4th stocks for a total of 5 + 2 + 3 = 10.
Next year, sell all three stocks for a total of 8 + 3 + 5 = 16.
The profit you made is 16 - 10 = 6.
It can be shown that the maximum profit you can make is 6.

```

**Example 2:**

```
Input: present = [2,2,5], future = [3,4,10], budget = 6
Output: 5
Explanation: The only possible way to maximize your profit is to:
Buy the 2nd stock, and make a profit of 10 - 5 = 5.
It can be shown that the maximum profit you can make is 5.

```

**Example 3:**

```
Input: present = [3,3,12], future = [0,3,15], budget = 10
Output: 0
Explanation: One possible way to maximize your profit is to:
Buy the 1st stock, and make a profit of 3 - 3 = 0.
It can be shown that the maximum profit you can make is 0.

```

**Constraints:**

- `n == present.length == future.length`
- `1 <= n <= 1000`
- `0 <= present[i], future[i] <= 100`
- `0 <= budget <= 1000`

```cpp
class Solution {
public:
    int maximumProfit(vector<int>& present, vector<int>& future, int budget) {
        int m=present.size(); if(m==0) {return 0;}
        vector<int> profits(m,0);
        for(int i=0; i<m; i++){
            profits[i]=future[i]-present[i];
        }
        //back pack - sz and price
        //present: cur sz
        //profits: cur val
        //bugdge: pack sz
        //return max val
        int n=budget;
        vector<vector<int>> f(m+1,vector<int>(n+1,0));
        //init: 1st row 0
        for(int i=1;i<m+1;i++){
            int curSz=present[i-1];
            int curVal=profits[i-1];
            for(int j=0;j<n+1;j++){
                int take=(j-curSz>=0)?curVal+f[i-1][j-curSz] : 0;
                int nonTake=f[i-1][j];
                f[i][j]=max(take, nonTake);
            }
        }
        //return max of f.b
        return f.back().back();
    }
};
```