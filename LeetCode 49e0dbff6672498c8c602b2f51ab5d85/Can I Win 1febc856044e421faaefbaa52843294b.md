# Can I Win

#: 464
Difficult: Medium
Tags: Bit Manipulation, BitMask, DP, Game, Math, recursion
link: https://leetcode.com/problems/can-i-win/description/
Priority: High
Status: No Idea At First, toSummarize
Created time: June 19, 2023 10:00 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

In the "100 game" two players take turns adding, to a running total, any integer from `1` to `10`. The player who first causes the running total to **reach or exceed** 100 wins.

What if we change the game so that players **cannot** re-use integers?

For example, two players might take turns drawing from a common pool of numbers from 1 to 15 without replacement until they reach a total >= 100.

Given two integers `maxChoosableInteger` and `desiredTotal`, return `true` if the first player to move can force a win, otherwise, return `false`. Assume both players play **optimally**.

**Example 1:**

```
Input: maxChoosableInteger = 10, desiredTotal = 11
Output: false
Explanation:
No matter which integer the first player choose, the first player will lose.
The first player can choose an integer from 1 up to 10.
If the first player choose 1, the second player can only choose integers from 2 up to 10.
The second player will win by choosing 10 and get a total = 11, which is >= desiredTotal.
Same with other integers chosen by the first player, the second player will always win.

```

**Example 2:**

```
Input: maxChoosableInteger = 10, desiredTotal = 0
Output: true

```

**Example 3:**

```
Input: maxChoosableInteger = 10, desiredTotal = 1
Output: true

```

**Constraints:**

- `1 <= maxChoosableInteger <= 20`
- `0 <= desiredTotal <= 300`

```cpp
class Solution {
public:
    int dp[1<<21];
    bool canIWin(int maxChoosableInteger, int desiredTotal) {
        //dfs的assumption是b如果不能赢那a一定赢，这个assumption是错的。当desiredTotal太大的时候，无论怎么选，a和b都不能赢，所以加上上面两行的目的是catch这种情况。
        int totalSum= (1+maxChoosableInteger)*maxChoosableInteger/2;
        if(totalSum<desiredTotal) return false;

        return dfs(0,0,maxChoosableInteger,desiredTotal);
    }
    //sum relied on state acutally
    bool dfs(int state, int sum, int maxChoosableInteger, int desiredTotal){
        if(dp[state]==2) return true;//win
        if(dp[state]==1) return false;//false
        for(int i=1; i<=maxChoosableInteger; i++){
            if((state>>i)&1) continue;
            if(sum+i>=desiredTotal) {
                dp[state]=2;
                return true;
            }
            
            if(dfs(state|(1<<i), sum+i, maxChoosableInteger, desiredTotal)==false){
                dp[state]=2;
                return true;
            }
        }
        dp[state]=1;
        return false;
    }
};

/*
A win => there exist a solution that A take a number, B will definitely lose

dfs(set of all number, sum=0, maxChoosableInteger,desiredTotal)
    for available number from set(bit mask state)
        if dfs(mask-num, sum+num, ..,..) = false -> return true
    return false
*/
```