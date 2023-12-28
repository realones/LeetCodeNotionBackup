# Maximum Length of Subarray With Positive Product

#: 1567
Difficult: Medium
Tags: DP, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/maximum-length-of-subarray-with-positive-product/description/
Priority: Medium
Status: HardToImplement
Created time: June 27, 2023 9:55 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an array of integers `nums`, find the maximum length of a subarray where the product of all its elements is positive.

A subarray of an array is a consecutive sequence of zero or more values taken out of that array.

Return *the maximum length of a subarray with positive product*.

**Example 1:**

```
Input: nums = [1,-2,-3,4]
Output: 4
Explanation: The array nums already has a positive product of 24.

```

**Example 2:**

```
Input: nums = [0,1,-2,-3,-4]
Output: 3
Explanation: The longest subarray with positive product is [1,-2,-3] which has a product of 6.
Notice that we cannot include 0 in the subarray since that'll make the product 0 which is not positive.
```

**Example 3:**

```
Input: nums = [-1,-2,-3,0,1]
Output: 2
Explanation: The longest subarray with positive product is [-1,-2] or [-2,-3].

```

**Constraints:**

- `1 <= nums.length <= 105`
- `109 <= nums[i] <= 109`

```cpp
class Solution {
public:
    int getMaxLen(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        vector<vector<int>> dp(n+1, vector<int>(2,-1));//-1: invalid, 0:pos, 1:neg
        int res=0;
        for(int i=1; i<n+1; i++){
            int cur=nums[i-1];
            if(cur==0) {
                dp[i][0] = -1;
                dp[i][1] = -1;
                continue;
            }
            //pos
            dp[i][0]= cur>0 ?
                            (dp[i-1][0]!=-1 ? dp[i-1][0]+1 : 1) : //cur is +
                            (dp[i-1][1]!=-1 ? dp[i-1][1]+1 : -1); //cur is -
            //neg
            dp[i][1]= cur>0 ?
                            (dp[i-1][1]!=-1 ? dp[i-1][1]+1 : -1) : //cur is +
                            (dp[i-1][0]!=-1 ? dp[i-1][0]+1 : 1); //cur is -
            res=max(res, dp[i][0]);
        }
        return res;
    }
};

/*
state: till cur, neg, longest
state: till cur, pos, longest

pos from
    if +
        prePos
            valid
                +1
            invalid
                =1
        preNeg
            valid
                NA
            invalid
                NA
    if -
        prePos
            valid
                NA
            invalid
                NA
        preNeg
            valid
                +1
            invalid
                NA

neg from
    if +
        prePos
            valid
                NA
            invalid
                NA
        preNeg
            valid
                +1
            invalid
                NA
    if -
        prePos
            valid
                +1
            invalid
                =1
        preNeg
            valid
                NA
            invalid
                NA
*/
```