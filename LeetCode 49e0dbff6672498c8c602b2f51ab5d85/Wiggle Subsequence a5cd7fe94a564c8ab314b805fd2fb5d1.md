# Wiggle Subsequence

#: 376
Difficult: Medium
Tags: DP, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/wiggle-subsequence/
Priority: High
Status: Dig More, More Solution, No Idea At First
Created time: November 18, 2022 12:48 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

A **wiggle sequence** is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.

- For example, `[1, 7, 4, 9, 2, 5]` is a **wiggle sequence** because the differences `(6, -3, 5, -7, 3)` alternate between positive and negative.
- In contrast, `[1, 4, 7, 2, 5]` and `[1, 7, 4, 5, 5]` are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.

A **subsequence** is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.

Given an integer array `nums`, return *the length of the longest **wiggle subsequence** of* `nums`.

**Example 1:**

```
Input: nums = [1,7,4,9,2,5]
Output: 6
Explanation: The entire sequence is a wiggle sequence with differences (6, -3, 5, -7, 3).

```

**Example 2:**

```
Input: nums = [1,17,5,10,13,15,10,5,16,8]
Output: 7
Explanation: There are several subsequences that achieve this length.
One is [1, 17, 10, 13, 10, 16, 8] with differences (16, -7, 3, -3, 6, -8).

```

**Example 3:**

```
Input: nums = [1,2,3,4,5,6,7,8,9]
Output: 2

```

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`

Solution:

1. States:
    1. inc: end with cur idx and increasing from pre
    2. dec: end with cur idx and decreasing from pre
2. Action(from pre to cur):
    1. increase
        1. inc=dec+1, dec=dec
    2. decrease
        1. dec=inc+1, inc=inc
    3. equal:
        1. inc=inc, dec=dec

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int inc=1,dec=1;
        for(int i=1;i<nums.size();i++){
            if(nums[i-1]<nums[i]){//inc
                inc=dec+1;
            }
            else if(nums[i-1]>nums[i]){//dec
                dec=inc+1;
            }
        }
        return max(dec,inc);
    }
};
```