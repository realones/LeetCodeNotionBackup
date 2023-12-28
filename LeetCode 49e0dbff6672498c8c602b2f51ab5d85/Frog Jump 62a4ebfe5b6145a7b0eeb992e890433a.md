# Frog Jump

#: 403
Difficult: Hard
Tags: Array, DP, GoForth, buildFutureResults
link: https://leetcode.com/problems/frog-jump/
Priority: Medium
Created time: August 19, 2022 5:14 PM
Last edited time: October 22, 2023 12:10 AM
practicedTimes: 1
source: jiuzhang, 算初DP

A frog is crossing a river. The river is divided into some number of units, and at each unit, there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of `stones`' positions (in units) in sorted **ascending order**, determine if the frog can cross the river by landing on the last stone. Initially, the frog is on the first stone and assumes the first jump must be `1` unit.

If the frog's last jump was `k` units, its next jump must be either `k - 1`, `k`, or `k + 1` units. The frog can only jump in the forward direction.

**Example 1:**

```
Input: stones = [0,1,3,5,6,8,12,17]
Output: true
Explanation: The frog can jump to the last stone by jumping 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 2 units to the 4th stone, then 3 units to the 6th stone, 4 units to the 7th stone, and 5 units to the 8th stone.

```

**Example 2:**

```
Input: stones = [0,1,2,3,4,8,9,11]
Output: false
Explanation: There is no way to jump to the last stone as the gap between the 5th and 6th stone is too large.

```

**Constraints:**

- `2 <= stones.length <= 2000`
- `0 <= stones[i] <= 231 - 1`
- `stones[0] == 0`
- `stones` is sorted in a strictly increasing order.

```cpp
class Solution {
public:
    bool canCross(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return false;}
        //stone location - incoming strengthes
        unordered_map<int, unordered_set<int>> f;
        for(auto num:nums){
            f[num]=unordered_set<int>();
        }
        //use 0 so can canculate 1
        f[0].insert(0);
        for(int i=0;i<n;i++){
            int location=nums[i];
            for(auto incomingStrength: f[location]){
                for(int newStr=incomingStrength-1; newStr<=incomingStrength+1;newStr++){
                    int newLocation=location+newStr;
                    if(f.count(newLocation)){
                        f[newLocation].insert(newStr);
                    }
                }
            }
        }
        return f[nums[n-1]].size()>0;
    }
};
```

```cpp
class Solution {
public:
    bool canCross(vector<int>& stones) {
        int n=stones.size();
        unordered_map<int,int> pos_idx;
        for(int i=0; i<n; i++){
            pos_idx[stones[i]]=i;
        }
        vector<unordered_set<int>> dp(n);
        dp[0]={0};
        for(int i=0; i<n; i++){
            for(int str: dp[i]){
                for(int nxtStr=str-1;nxtStr<=str+1;nxtStr++){
                    int nxtPos=pos_idx[stones[i]+nxtStr];
                    dp[nxtPos].insert(nxtStr);
                }
            }
        }
        return !dp[n-1].empty();
    }
};

/*
stones:
0 1 3 5 6 8 12 17

init: on stones[0]=0, 1st jump_str=1

lastJump=k -> nxt jump= k-1~k+1
================================
n:2~2000
val: 0~INT_MAX

f(i): strenghs
fori:0~n-1
    f(i) -> f(i+k)
f(0) = {1}

return f(n-1) not empty
*/
```