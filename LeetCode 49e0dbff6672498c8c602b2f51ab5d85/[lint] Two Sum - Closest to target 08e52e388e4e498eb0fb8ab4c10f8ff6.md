# [lint] Two Sum - Closest to target

#: 533
Difficult: Medium
Tags: 2pt_DiffDir
link: https://www.lintcode.com/problem/two-sum-closest-to-target
Priority: Pass
Created time: August 6, 2022 11:59 PM
Last edited time: October 19, 2023 5:53 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

Given an array `nums` of *n* integers, find two integers in *nums* such that the sum is closest to a given number, *target*.

Return the absolute value of difference between the sum of the two integers and the target.

Example:
**Example1**

```
Input:  nums = [-1, 2, 1, -4] and target = 4
Output: 1
Explanation:
The minimum difference is 1. (4 - (2 + 1) = 1).

```

**Example2**

```
Input:  nums = [-1, -1, -1, -4] and target = 4
Output: 6
Explanation:
The minimum difference is 6. (4 - (- 1 - 1) = 6).

```

Challenge
Do it in O(nlogn) time complexity.

```cpp
public class Solution {
    /**
     * @param nums an integer array
     * @param target an integer
     * @return the difference between the sum and the target
     */
    public int twoSumClosest(int[] nums, int target) {
        if (nums == null || nums.length < 2) {
            return -1;
        }

        Arrays.sort(nums);

        int left = 0, right = nums.length - 1;
        int diff = Integer.MAX_VALUE;

        while (left < right) {
            if (nums[left] + nums[right] < target) {
                diff = Math.min(diff, target - nums[left] - nums[right]);
                left++;
            } else {
                diff = Math.min(diff, nums[left] + nums[right] - target);
                right--;
            }
        }

        return diff;
    }
}
```