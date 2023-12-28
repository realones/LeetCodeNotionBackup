# Knight Dialer

#: 935
Difficult: Medium
Tags: Graph, Matrix, dp-TimeSeqFromLastOne-Power, dp-bfs-stepByStep
link: https://leetcode.com/problems/knight-dialer/description/
Priority: Low
Status: toSummarize
Created time: June 28, 2023 12:45 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Count Vowels Permutation (Count%20Vowels%20Permutation%20dda110cbbdd045dcb165eceb07f72d7b.md), Number of Ways to Stay in the Same Place After Some Steps (Number%20of%20Ways%20to%20Stay%20in%20the%20Same%20Place%20After%20Som%2091772bfb41a84c87b0a4d56f1f7f74bd.md)

The chess knight has a **unique movement**, it may move two squares vertically and one square horizontally, or two squares horizontally and one square vertically (with both forming the shape of an **L**). The possible movements of chess knight are shown in this diagaram:

A chess knight can move as indicated in the chess diagram below:

![https://assets.leetcode.com/uploads/2020/08/18/chess.jpg](https://assets.leetcode.com/uploads/2020/08/18/chess.jpg)

We have a chess knight and a phone pad as shown below, the knight **can only stand on a numeric cell** (i.e. blue cell).

![https://assets.leetcode.com/uploads/2020/08/18/phone.jpg](https://assets.leetcode.com/uploads/2020/08/18/phone.jpg)

Given an integer `n`, return how many distinct phone numbers of length `n` we can dial.

You are allowed to place the knight **on any numeric cell** initially and then you should perform `n - 1` jumps to dial a number of length `n`. All jumps should be **valid** knight jumps.

As the answer may be very large, **return the answer modulo** `109 + 7`.

**Example 1:**

```
Input: n = 1
Output: 10
Explanation: We need to dial a number of length 1, so placing the knight over any numeric cell of the 10 cells is sufficient.

```

**Example 2:**

```
Input: n = 2
Output: 20
Explanation: All the valid number we can dial are [04, 06, 16, 18, 27, 29, 34, 38, 40, 43, 49, 60, 61, 67, 72, 76, 81, 83, 92, 94]

```

**Example 3:**

```
Input: n = 3131
Output: 136006598
Explanation: Please take care of the mod.

```

**Constraints:**

- `1 <= n <= 5000`

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int knightDialer(int n) {
        unordered_map<int,unordered_set<int>> g;
        g[0].insert({4,6});
        g[1].insert({6,8});
        g[2].insert({7,9});
        g[3].insert({4,8});
        g[4].insert({3,9,0});
        g[5].insert({});
        g[6].insert({1,7,0});
        g[7].insert({2,6});
        g[8].insert({1,3});
        g[9].insert({2,4});

        vector<vector<ll>> dp(n,vector<ll>(10,0));
        for(int j=0; j<10; j++){ dp[0][j]=1; }
        for(int i=0;i<n-1;i++){//i->i+1
            for(int j=0;j<10;j++){
                for(int nj: g[j]){
                    dp[i+1][nj]+=dp[i][j];
                    dp[i+1][nj]%=M;
                }
            }
        }
        return accumulate(dp.back().begin(), dp.back().end(),0LL)%M;
    }
};

//if input n is big, use binary lifting
/*
graph
*/
```