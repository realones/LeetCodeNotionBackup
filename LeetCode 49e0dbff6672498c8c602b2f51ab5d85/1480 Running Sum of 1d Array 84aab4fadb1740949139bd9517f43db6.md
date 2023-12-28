# 1480. Running Sum of 1d Array

#: 1480
Difficult: Easy
Tags: Array, Prefix
link: https://leetcode.com/problems/running-sum-of-1d-array/description/
Priority: Pass
Created time: December 24, 2023 11:00 PM
Last edited time: December 24, 2023 11:00 PM
source: googleFreq

Given an array `nums`. We define a running sum of an array as `runningSum[i] = sum(nums[0]…nums[i])`.

Return the running sum of `nums`.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [1,3,6,10]
Explanation: Running sum is obtained as follows: [1, 1+2, 1+2+3, 1+2+3+4].
```

**Example 2:**

```
Input: nums = [1,1,1,1,1]
Output: [1,2,3,4,5]
Explanation: Running sum is obtained as follows: [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1].
```

**Example 3:**

```
Input: nums = [3,1,2,10,1]
Output: [3,4,6,16,17]

```

**Constraints:**

- `1 <= nums.length <= 1000`
- `10^6 <= nums[i] <= 10^6`

```cpp
//cp from Editorial
class Solution {
public:
    vector<int> runningSum(vector<int> &nums) {
        for (int i = 1; i < nums.size(); i++) {
            // Result at index `i` is sum of result at `i-1` and element at `i`.
            nums[i] += nums[i - 1];
        }
        return nums;
    }
};
```