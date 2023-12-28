# Subarrays with K different Integers

#: 992
Difficult: Hard
Tags: Array, Hash, Sliding Window
link: https://leetcode.com/problems/subarrays-with-k-different-integers/description/
Priority: Medium
Status: HardToImplement
Created time: June 14, 2023 6:21 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wisdompeak
related: [lint] Subarray Sum II (%5Blint%5D%20Subarray%20Sum%20II%20836d41ad6f404dca85726a0a0802019e.md)

Given an integer array `nums` and an integer `k`, return *the number of **good subarrays** of* `nums`.

A **good array** is an array where the number of different integers in that array is exactly `k`.

- For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

```
Input: nums = [1,2,1,2,3], k = 2
Output: 7
Explanation: Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]

```

**Example 2:**

```
Input: nums = [1,2,1,3,4], k = 3
Output: 3
Explanation: Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].

```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `1 <= nums[i], k <= nums.length`

comment:

- did similar question [lint - Subarray Sum II] before so know to maintain 2 windows instead of just one, if just maintain one, you don’t know move l or r.
- implementation is a little bit not straightforward
- seems some how related to PrefixSum

Solution:

```cpp
class Solution {
public:
    int subarraysWithKDistinct(vector<int>& nums, int k) {
        int n=nums.size();//n: 1~2e4
        int res=0;
        unordered_map<int,int> mp1, mp2;
        int sz1=0, sz2=0;
        //l1...l2...r
        for(int l1=0, l2=0, r=0; r<n; r++){
            //add r -> update mp
            mp1[nums[r]]++;
            if(mp1[nums[r]]==1) sz1++;
            mp2[nums[r]]++;
            if(mp2[nums[r]]==1) sz2++;
            //while invalid trim l1 to first valid - largest valid window
            while(sz1>k){
                mp1[nums[l1]]--;
                if(mp1[nums[l1]]==0) sz1--;
                l1++;
            }
            //while valid trim l2 to first invalid - largest invalid window after the valid part
            //to improve to start max(l1,l2) and copy mp1 if needed
            while(sz2>=k){
                mp2[nums[l2]]--;
                if(mp2[nums[l2]]==0) sz2--;
                l2++;
            }
            //udpate res
            if(sz1==k && sz2==k-1) res+=l2-l1;
        }
        return res;
    }
};

/*
sliding window
l1 l2 r
    for each r, maintain left most l1 and right most l2

maintain mp: int - cnt
maintain mp - sz (how many int has freq > 0)
*/
```