# Monotonic Array

#: 896
Difficult: Easy
Tags: Array
link: https://leetcode.com/problems/monotonic-array/description/
Priority: Pass
Created time: December 24, 2023 11:39 PM
Last edited time: December 24, 2023 11:39 PM
source: googleFreq

An array is **monotonic** if it is either monotone increasing or monotone decreasing.

An array `nums` is monotone increasing if for all `i <= j`, `nums[i] <= nums[j]`. An array `nums` is monotone decreasing if for all `i <= j`, `nums[i] >= nums[j]`.

Given an integer array `nums`, return `true` *if the given array is monotonic, or* `false` *otherwise*.

**Example 1:**

```
Input: nums = [1,2,2,3]
Output: true

```

**Example 2:**

```
Input: nums = [6,5,4,4]
Output: true

```

**Example 3:**

```
Input: nums = [1,3,2]
Output: false

```

**Constraints:**

- `1 <= nums.length <= 105`
- `105 <= nums[i] <= 105`

```java
//cp from Editorial
class Solution {
    public boolean isMonotonic(int[] A) {
        boolean increasing = true;
        boolean decreasing = true;
        for (int i = 0; i < A.length - 1; ++i) {
            if (A[i] > A[i+1])
                increasing = false;
            if (A[i] < A[i+1])
                decreasing = false;
        }

        return increasing || decreasing;
    }
}
```