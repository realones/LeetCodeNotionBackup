# Last Stone Weight II

#: 1049
Difficult: Medium
Tags: dp-backpack
link: https://leetcode.com/problems/last-stone-weight-ii/
Priority: Medium
Status: dup
Created time: January 1, 2023 11:53 AM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路
related: Target Sum (Target%20Sum%208a85def49fcf426f820e294f30d67c62.md)

You are given an array of integers `stones` where `stones[i]` is the weight of the `ith` stone.

We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights `x` and `y` with `x <= y`. The result of this smash is:

- If `x == y`, both stones are destroyed, and
- If `x != y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

At the end of the game, there is **at most one** stone left.

Return *the smallest possible weight of the left stone*. If there are no stones left, return `0`.

**Example 1:**

```
Input: stones = [2,7,4,1,8,1]
Output: 1
Explanation:
We can combine 2 and 4 to get 2, so the array converts to [2,7,1,8,1] then,
we can combine 7 and 8 to get 1, so the array converts to [2,1,1,1] then,
we can combine 2 and 1 to get 1, so the array converts to [1,1,1] then,
we can combine 1 and 1 to get 0, so the array converts to [1], then that's the optimal value.

```

**Example 2:**

```
Input: stones = [31,26,33,21,40]
Output: 5

```

**Constraints:**

- `1 <= stones.length <= 30`
- `1 <= stones[i] <= 100`

Solution offset version:

```jsx
/*
similar to 494.Target-Sum
we can add + or - in front of each num
*/
class Solution {
public:
    int lastStoneWeightII(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        int sum=accumulate(nums.begin(), nums.end(),0);
        int offset=sum;
        
        nums.insert(nums.begin(),0);
        vector<vector<bool>> dp(n+1,vector<bool>(sum+1+offset,false));
        dp[0][0+offset]=true;
        
        for(int i=1;i<n+1;i++){
            int curVal=nums[i];
            for(int s=-sum;s<=sum;s++){
                dp[i][s+offset]=
                    ((-sum<=s-curVal && s-curVal<=sum)? dp[i-1][s-curVal+offset]: false)
                    ||
                    ((-sum<=s+curVal && s+curVal<=sum)? dp[i-1][s+curVal+offset]: false)
                    ;
            }
        }
        
        int res=-1;
        for(int s=0;s<=sum;s++){
            if(dp.back()[s+offset]) {res=s; break;}
        }
        
        return res;
    }
};
```

Solution - mp version

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int n=stones.size();
        int total=accumulate(stones.begin(), stones.end(),0);
        //dp[first ith items:0~n][weight:0~total]
        vector<unordered_map<int,bool>> dp(n+1);
        //init: false, dp[0][0]=1
        dp[0][0]=true;
        function f=[&](int i,int j)->bool{
            return (j<0 || j>total)?false:dp[i][j];
        };
        for(int i=1; i<=n; i++){
            int w=stones[i-1];
            for(int j=0; j<=total; j++){
                dp[i][j]= 
                    f(i-1,j-w) || f(i-1,w+j) || f(i-1,w-j);
            }
        }
        for(int j=0; j<=total; j++){
            if(dp.back()[j]==true) return j;
        }
        return -1;
    }
};
/*
for each stone, can
    accumulate to remaining weight from left
        dp[i][j]= dp[i-1][j'], j'+w=j => j'=j-w
    fight with remaining weight from left
        dp[i][j]= dp[i-1][j'], fight(j',w)=j => j'= w+j || j'=w-j
*/
```