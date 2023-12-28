# Majority Element II

#: 229
Difficult: Medium
Tags: Array, Counting, Hash, Sort
link: https://leetcode.com/problems/majority-element-ii/
Priority: High
Status: No Idea At First
Created time: October 5, 2023 1:47 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

**Example 1:**

```
Input: nums = [3,2,3]
Output: [3]

```

**Example 2:**

```
Input: nums = [1]
Output: [1]

```

**Example 3:**

```
Input: nums = [1,2]
Output: [1,2]

```

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `109 <= nums[i] <= 109`

**Follow up:** Could you solve the problem in linear time and in `O(1)` space?

Solution:

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int can1=0,can2=0,cnt1=0,cnt2=0;
        vector<int> res;
        for(auto num:nums){
            if(num==can1){cnt1++;}
            else if(num==can2){cnt2++;}
            else if(cnt1==0){can1=num;cnt1=1;}
            else if(cnt2==0){can2=num;cnt2=1;}
            else{cnt1--;cnt2--;}
        }
        
        cnt1=0,cnt2=0;
        for(auto num:nums){
            if(num==can1){cnt1++;}
            else if(num==can2){cnt2++;}
        }
        
        if(cnt1>nums.size()/3){res.push_back(can1);}
        if(cnt2>nums.size()/3){res.push_back(can2);}
        return res;
    }
};
```