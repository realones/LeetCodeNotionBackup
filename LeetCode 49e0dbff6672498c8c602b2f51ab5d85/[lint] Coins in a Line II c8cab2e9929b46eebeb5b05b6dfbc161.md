# [lint] Coins in a Line II

#: 395
Difficult: Medium
Tags: DP, Game
link: https://www.lintcode.com/problem/395/
Priority: Medium
Created time: September 10, 2022 10:49 AM
Last edited time: October 30, 2023 12:32 AM
practicedTimes: 1
source: jiuzhang, 算高DP上

Description

There are n coins with different value in a line. Two players take turns to take one or two coins from **left side** until there are no more coins left. The player who take the coins with the most value wins.

Could you please decide the **first** player will win or lose?

If the first player wins, return `true`, otherwise return `false`.

Example

**Example 1:**

```
Input: [1, 2, 2]
Output: true
Explanation: The first player takes 2 coins.

```

**Example 2:**

```
Input: [1, 2, 4]
Output: false
Explanation: Whether the first player takes 1 coin or 2, the second player will gain more value.

```

comment：可以reverse一下nums

evernote —

```cpp
class Solution {
public:
    /*
     * @param values: a vector of integers
     * @return: a boolean which equals to true if the first player will win
     */
    bool firstWillWin(vector<int> &values) {
        int n=values.size();
        if(n==0) return false;
        
        int sum=0;
        vector<int> f(n+1,0);
        sum+=values[n-1];f[1]=sum;
        sum+=values[n-2];f[2]=sum;
        for(int i=3;i<=n;i++){
            sum+=values[n-i];
            f[i]=max(sum-f[i-1],sum-f[i-2]);
        }
        return f[n]*2>sum;
    }
};
```