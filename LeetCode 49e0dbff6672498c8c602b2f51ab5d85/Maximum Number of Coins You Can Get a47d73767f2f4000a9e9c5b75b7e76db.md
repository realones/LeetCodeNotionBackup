# Maximum Number of Coins You Can Get

#: 1561
Difficult: Medium
Tags: Array, Game, Greedy, Math, Sort
link: https://leetcode.com/problems/maximum-number-of-coins-you-can-get/
Priority: Medium
Created time: November 26, 2023 11:56 PM
Last edited time: November 26, 2023 11:57 PM
source: leetcode_daily

There are `3n` piles of coins of varying size, you and your friends will take piles of coins as follows:

- In each step, you will choose **any** `3` piles of coins (not necessarily consecutive).
- Of your choice, Alice will pick the pile with the maximum number of coins.
- You will pick the next pile with the maximum number of coins.
- Your friend Bob will pick the last pile.
- Repeat until there are no more piles of coins.

Given an array of integers `piles` where `piles[i]` is the number of coins in the `ith` pile.

Return the maximum number of coins that you can have.

**Example 1:**

```
Input: piles = [2,4,1,2,7,8]
Output: 9
Explanation:Choose the triplet (2, 7, 8), Alice Pick the pile with 8 coins, you the pile with7 coins and Bob the last one.
Choose the triplet (1, 2, 4), Alice Pick the pile with 4 coins, you the pile with2 coins and Bob the last one.
The maximum number of coins which you can have are: 7 + 2 = 9.
On the other hand if we choose this arrangement (1,2, 8), (2,4, 7) you only get 2 + 4 = 6 coins which is not optimal.

```

**Example 2:**

```
Input: piles = [2,4,5]
Output: 4

```

**Example 3:**

```
Input: piles = [9,8,7,6,5,1,2,3,4]
Output: 18

```

**Constraints:**

- `3 <= piles.length <= 105`
- `piles.length % 3 == 0`
- `1 <= piles[i] <= 104`

```cpp
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        sort(nums.begin(), nums.end(), greater<>());
        int res=0;
        for(int l=0,r=n-1; l<r; l+=2, r--){
            //l: alice, l+1: me, r: bob
            res+=nums[l+1];
        }
        return res;
    }
};

/*
nums:
    len: 3~1e5, len%3=0
    val: 1~1e4
================================
dp[a set of nums]: max you get from those nums
dp[set of nums] = combination -> TLER
return: dp[all nums]
================================
strategry:
    pick a==b c=smallest
    the largest always lose to Alice
    we can at most leverage it to get the 2nd largest ones
    give the smallest one to Bob
    greedy
*/
```