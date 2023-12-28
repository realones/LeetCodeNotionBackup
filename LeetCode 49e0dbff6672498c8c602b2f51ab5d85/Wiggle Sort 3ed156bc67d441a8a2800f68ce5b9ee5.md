# Wiggle Sort

#: 280
Difficult: Medium
Tags: Greedy, Sort
link: https://leetcode.com/problems/wiggle-sort/
Priority: Pass
Created time: September 22, 2022 12:17 PM
Last edited time: October 31, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算高FollowUp

Given an integer array `nums`, reorder it such that `nums[0] <= nums[1] >= nums[2] <= nums[3]...`.

You may assume the input array always has a valid answer.

**Example 1:**

```
Input: nums = [3,5,2,1,6,4]
Output: [3,5,1,6,2,4]
Explanation: [1,6,2,5,3,4] is also accepted.

```

**Example 2:**

```
Input: nums = [6,6,5,6,3,8]
Output: [6,6,5,6,3,8]

```

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `0 <= nums[i] <= 104`
- It is guaranteed that there will be an answer for the given input `nums`.

**Follow up:** Could you solve the problem in `O(n)` time complexity?

```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return;}
        for(int i=1;i<n;i++){
            if(i%2){//inc
                if(nums[i-1]>nums[i]){swap(nums[i-1],nums[i]);}
            }
            else{//dec
                if(nums[i-1]<nums[i]){swap(nums[i-1],nums[i]);}
            }
        }
    }
};
```