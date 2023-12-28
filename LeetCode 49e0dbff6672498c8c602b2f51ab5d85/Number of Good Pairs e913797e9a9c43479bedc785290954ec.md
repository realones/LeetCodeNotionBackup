# Number of Good Pairs

#: 1512
Difficult: Easy
Tags: Array, Counting, Hash, Math
link: https://leetcode.com/problems/number-of-good-pairs/description/
Priority: Pass
Created time: December 24, 2023 10:59 PM
Last edited time: December 24, 2023 10:59 PM
source: googleFreq

Given an array of integers `nums`, return *the number of **good pairs***.

A pair `(i, j)` is called *good* if `nums[i] == nums[j]` and `i` < `j`.

**Example 1:**

```
Input: nums = [1,2,3,1,1,3]
Output: 4
Explanation: There are 4 good pairs (0,3), (0,4), (3,4), (2,5) 0-indexed.

```

**Example 2:**

```
Input: nums = [1,1,1,1]
Output: 6
Explanation: Each pair in the array aregood.

```

**Example 3:**

```
Input: nums = [1,2,3]
Output: 0

```

**Constraints:**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

```cpp
//cp from Editorial
class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        unordered_map<int, int> counts;
        int ans = 0;
        
        for (int num: nums) {
            ans += counts[num];
            counts[num]++;
        }
        
        return ans;
    }
};
```