# Profitable Schemes

#: 879
Difficult: Hard
Tags: dp-backpack
link: https://leetcode.com/problems/profitable-schemes/
Priority: High
Status: Dig More
Created time: January 3, 2023 10:44 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路

There is a group of `n` members, and a list of various crimes they could commit. The `ith` crime generates a `profit[i]` and requires `group[i]` members to participate in it. If a member participates in one crime, that member can't participate in another crime.

Let's call a **profitable scheme** any subset of these crimes that generates at least `minProfit` profit, and the total number of members participating in that subset of crimes is at most `n`.

Return the number of schemes that can be chosen. Since the answer may be very large, **return it modulo** `109 + 7`.

**Example 1:**

```
Input: n = 5, minProfit = 3, group = [2,2], profit = [2,3]
Output: 2
Explanation: To make a profit of at least 3, the group could either commit crimes 0 and 1, or just crime 1.
In total, there are 2 schemes.
```

**Example 2:**

```
Input: n = 10, minProfit = 5, group = [2,3,5], profit = [6,7,8]
Output: 7
Explanation: To make a profit of at least 5, the group could commit any crimes, as long as they commit one.
There are 7 possible schemes: (0), (1), (2), (0,1), (0,2), (1,2), and (0,1,2).
```

**Constraints:**

- `1 <= n <= 100`
- `0 <= minProfit <= 100`
- `1 <= group.length <= 100`
- `1 <= group[i] <= 100`
- `profit.length == group.length`
- `0 <= profit[i] <= 100`

```jsx
class Solution {
public:
    int profitableSchemes(int n, int minProfit, vector<int>& group, vector<int>& profit) {
        int M=1e9+7;
        int profitSum=accumulate(profit.begin(),profit.end(),0);
        vector<vector<vector<int>>> dp(group.size()+1,vector<vector<int>>(n+1,vector<int>(minProfit+1,0)));
        dp[0][0][0]=1;
        for(int i=0; i<group.size(); i++){//from i to i+1
            int m=group[i];
            int p=profit[i];
            for(int j=0; j<n+1; j++){
                for(int k=0; k<minProfit+1; k++){
                    //not take
                    dp[i+1][j][k]+=dp[i][j][k];
                    dp[i+1][j][k]%=M;
                    //take
                    int nj=j+m; if(nj>n) continue;
                    int nk=min(minProfit,k+p);
                    dp[i+1][nj][nk]+=dp[i][j][k];
                    dp[i+1][nj][nk]%=M;
                }
            }
        }
        
        int res=0;
        for(int j=0; j<n+1; j++){
            res+=dp.back()[j].back();
            res%=M;
        }
        return res;
    }
};

/*
profit[i] - all crimes you picked >=minProfit
group[i] - how much people required, all crimes you picked sum of group[k] < n
combination of crimes

return dp [crimeCnt, x people, totalProfit>=minprofit] = combination cnt of crimes

*/
```