# Minimum Replacements to Sort the Array

#: 2366
Difficult: Hard
Tags: Array, Greedy, Math
link: https://leetcode.com/problems/minimum-replacements-to-sort-the-array/
Priority: Medium
Created time: August 29, 2023 7:29 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given a **0-indexed** integer array `nums`. In one operation you can replace any element of the array with **any two** elements that **sum** to it.

- For example, consider `nums = [5,6,7]`. In one operation, we can replace `nums[1]` with `2` and `4` and convert `nums` to `[5,2,4,7]`.

Return *the minimum number of operations to make an array that is sorted in **non-decreasing** order*.

**Example 1:**

```
Input: nums = [3,9,3]
Output: 2
Explanation: Here are the steps to sort the array in non-decreasing order:
- From [3,9,3], replace the 9 with 3 and 6 so the array becomes [3,3,6,3]
- From [3,3,6,3], replace the 6 with 3 and 3 so the array becomes [3,3,3,3,3]
There are 2 steps to sort the array in non-decreasing order. Therefore, we return 2.

```

**Example 2:**

```
Input: nums = [1,2,3,4,5]
Output: 0
Explanation: The array is already in non-decreasing order. Therefore, we return 0.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`

comment: wow, i figure it out pretty fast by myself :)

```cpp
using ll=long long;

class Solution {
public:
    long long minimumReplacement(vector<int>& nums) {
        ll cnt=0;
        for(int i=nums.size()-2; i>=0; i--){
            if(nums[i] <= nums[i+1]) continue;
            int pieces = (nums[i]/nums[i+1]) + (nums[i]%nums[i+1]>0);
            cnt+=pieces-1;
            nums[i] = nums[i]/pieces;
        }
        return cnt;
    }
};

/*
from back to front
3,9,3
    3 keep same
  9 to split small elements to <=3  ->  split evenly to stop one element too small

e.g.
  9  split to <=3: 9/3=3 <=3
  10 split to <=3: 10/3=3.3 > 3, so (10/3)+(10%3>0) = 4 pieces, each one with 10/4 initially, then put some ones to some elements
  8  split to <=3: (8/3)+(8%3>0) = 3 pieces

  => split to (curNum/nxtNum) + (curNum%nxtNum>0) pieces
  then let's curNum = curNum/pieces, then go to pervious number
*/
```