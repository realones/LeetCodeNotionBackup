# House Robber II

#: 213
Difficult: Medium
Tags: Array, DP, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/house-robber-ii/
Priority: Pass
Created time: September 9, 2022 12:14 PM
Last edited time: October 29, 2023 9:06 PM
practicedTimes: 1
source: HuaHua, jiuzhang, wp动归套路, 算高DP上

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

**Example 1:**

```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

```

**Example 2:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

```

**Example 3:**

```
Input: nums = [1,2,3]
Output: 3

```

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

[0..n-1] OR [n-1,0,..,n-2]

滚动数组优化一维

- 这类题目特点
    - f[i] = max(f[i-1], f[i-2] + A[i]); 由 f[i-1],f[i-2] 来决定状态
- 可以转化为
    - f[i%2] = max(f[(i-1)%2]和 f[(i-2)%2]) 由f[(i-1)%2]和 f[(i-2)%2] 来决定状态
- 观察我们需要保留的状态来确定模数

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        if(n==1) return nums[0];
        return max(rob(nums,0,n-2) , rob(nums,1,n-1));
    }
    
    int rob(vector<int>& nums, int start, int end){
        if(start==end) return nums[start];
        int n=nums.size();
        vector<int> f(2,0);
        f[start%2]=nums[start];
        f[(start+1)%2]=max(nums[start], nums[start+1]);
        for(int i=start+2;i<=end;i++){
            f[i%2]=max(f[(i-1)%2], f[(i-2)%2]+nums[i]);
        }
        return f[end%2];
    }
};
```