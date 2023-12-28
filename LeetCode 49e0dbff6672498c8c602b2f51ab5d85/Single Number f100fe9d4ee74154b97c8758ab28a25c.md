# Single Number

#: 136
Difficult: Easy
Tags: Bit Manipulation
link: https://leetcode.com/problems/single-number/
Priority: Low
Created time: June 6, 2023 11:07 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, LC_Top_Interview_150

Given a **non-empty** array of integers `nums`, every element appears *twice* except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**

```
Input: nums = [2,2,1]
Output: 1

```

**Example 2:**

```
Input: nums = [4,1,2,1,2]
Output: 4

```

**Example 3:**

```
Input: nums = [1]
Output: 1

```

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `3 * 104 <= nums[i] <= 3 * 104`
- Each element in the array appears twice except for one element which appears only once.

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res=0;
        for(auto x: nums){
            res^=x;
        }
        return res;
    }
};
```