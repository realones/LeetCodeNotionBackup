# [lint] Two Sum - Unique pairs

#: 587
Difficult: Medium
Tags: 2pt_DiffDir
link: https://www.lintcode.com/problem/two-sum-unique-pairs
Priority: Pass
Created time: August 6, 2022 11:59 PM
Last edited time: October 19, 2023 9:26 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

Given an array of integers, find how many `unique pairs` in the array such that their sum is equal to a specific target number. Please return the number of pairs.

Example:
**Example 1:**

```
Input: nums = [1,1,2,45,46,46], target = 47
Output: 2
Explanation:

1 + 46 = 47
2 + 45 = 47

```

**Example 2:**

```
Input: nums = [1,1], target = 2
Output: 1
Explanation:
1 + 1 = 2

```

```cpp
public class Solution {
    /**
     * @param nums an array of integer
     * @param target an integer
     * @return an integer
     */
    public int twoSum6(int[] nums, int target) {
        // Write your code here
        if (nums == null || nums.length < 2)
            return 0;

        Arrays.sort(nums);
        int cnt = 0;
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int v = nums[left] + nums[right];
            if (v == target) {
                cnt ++;
                left ++;
                right --;
                while (left < right && nums[right] == nums[right + 1])
                    right --;
                while (left < right && nums[left] == nums[left - 1])
                    left ++;
            } else if (v > target) {
                right --;
            } else {
                left ++;
            }
        }
        return cnt;
    }
}
```