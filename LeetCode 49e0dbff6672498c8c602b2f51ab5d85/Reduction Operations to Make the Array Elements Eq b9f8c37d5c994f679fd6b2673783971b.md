# Reduction Operations to Make the Array Elements Equal

#: 1887
Difficult: Medium
Tags: Array, Sort, simulation
link: https://leetcode.com/problems/reduction-operations-to-make-the-array-elements-equal/
Priority: Medium
Created time: November 18, 2023 10:48 PM
Last edited time: November 18, 2023 10:48 PM
source: leetcode_daily

Given an integer array `nums`, your goal is to make all elements in `nums` equal. To complete one operation, follow these steps:

1. Find the **largest** value in `nums`. Let its index be `i` (**0-indexed**) and its value be `largest`. If there are multiple elements with the largest value, pick the smallest `i`.
2. Find the **next largest** value in `nums` **strictly smaller** than `largest`. Let its value be `nextLargest`.
3. Reduce `nums[i]` to `nextLargest`.

Return *the number of operations to make all elements in* `nums` *equal*.

**Example 1:**

```
Input: nums = [5,1,3]
Output: 3
Explanation: It takes 3 operations to make all elements in nums equal:
1. largest = 5 at index 0. nextLargest = 3. Reduce nums[0] to 3. nums = [3,1,3].
2. largest = 3 at index 0. nextLargest = 1. Reduce nums[0] to 1. nums = [1,1,3].
3. largest = 3 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1].

```

**Example 2:**

```
Input: nums = [1,1,1]
Output: 0
Explanation: All elements in nums are already equal.

```

**Example 3:**

```
Input: nums = [1,1,2,2,3]
Output: 4
Explanation: It takes 4 operations to make all elements in nums equal:
1. largest = 3 at index 4. nextLargest = 2. Reduce nums[4] to 2. nums = [1,1,2,2,2].
2. largest = 2 at index 2. nextLargest = 1. Reduce nums[2] to 1. nums = [1,1,1,2,2].
3. largest = 2 at index 3. nextLargest = 1. Reduce nums[3] to 1. nums = [1,1,1,1,2].
4. largest = 2 at index 4. nextLargest = 1. Reduce nums[4] to 1. nums = [1,1,1,1,1].

```

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `1 <= nums[i] <= 5 * 104`

```cpp
class Solution {
public:
    int reductionOperations(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n=nums.size();//val,len: 1~5e4
        int cnt=0;
        for(int i=n-1;i>0;i--){
            if(nums[i]>nums[i-1]) cnt+=(n-i);//from i to n-1, dec
        }
        return cnt;
    }
};
/*
sort
if no dup:
idx->op
idx: 0 -> 0
idx: 1 -> 1
idx: 2 -> 2
idx: 3 -> 3
idx: i -> i
idx: n-1 -> n-1

so:
n(n-1)/2 - causedByDup
*/
```