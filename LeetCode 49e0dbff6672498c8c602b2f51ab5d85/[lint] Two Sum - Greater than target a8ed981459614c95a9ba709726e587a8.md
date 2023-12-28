# [lint] Two Sum - Greater than target

#: 443
Difficult: Medium
Tags: 2pt_DiffDir
link: https://www.lintcode.com/problem/two-sum-greater-than-target
Priority: Pass
Created time: August 6, 2022 11:59 PM
Last edited time: October 19, 2023 5:54 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

Given an array of integers, find how many pairs in the array such that their sum is bigger than a specific target number. Please return the number of pairs.

Example:
**Example 1:**

```
Input: [2, 7, 11, 15], target = 24
Output: 1
Explanation: 11 + 15 is the only pair.

```

**Example 2:**

```
Input: [1, 1, 1, 1], target = 1
Output: 6

```

Challenge
Do it in O(1) extra space and O(nlogn) time.

```cpp
public class Solution {
    /**
     * @param nums: an array of integer
     * @param target: an integer
     * @return: an integer
     */
    public int twoSum2(int[] nums, int target) {
        if (nums == null || nums.length < 2) {
            return 0;
        }

        Arrays.sort(nums);

        int left = 0, right = nums.length - 1;
        int count = 0;
        while (left < right) {
            if (nums[left] + nums[right] <= target) {
                left++;
            } else {
                count += right - left;
                right--;
            }
        }

        return count;
    }
}
```