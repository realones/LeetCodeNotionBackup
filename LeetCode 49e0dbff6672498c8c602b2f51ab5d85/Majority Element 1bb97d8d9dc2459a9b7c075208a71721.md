# Majority Element

#: 169
Difficult: Easy
Tags: Array, Bit Manipulation, Divide and Conquer, Hash
link: https://leetcode.com/problems/majority-element/
Priority: High
Status: No Idea At First
Created time: May 30, 2023 2:06 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1:**

```
Input: nums = [3,2,3]
Output: 3

```

**Example 2:**

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2

```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 5 * 1e4`
- `1e9 <= nums[i] <= 1e9`

**Follow-up:**

Could you solve the problem in linear time and in O(1)space?

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int cand=0,cnt=0;
        for(auto num:nums){
            if(cand==num) cnt++;
            else if(cnt==0){
                cand=num;
                cnt=1;
            }
            else cnt--;
        }
        return cand;
    }
};
```