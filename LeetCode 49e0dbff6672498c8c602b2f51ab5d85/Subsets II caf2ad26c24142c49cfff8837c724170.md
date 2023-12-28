# Subsets II

#: 90
Difficult: Medium
Tags: BackTracking, Combination
link: https://leetcode.com/problems/subsets-ii/
Priority: Low
Created time: July 28, 2022 12:37 AM
Last edited time: October 16, 2023 4:49 PM
practicedTimes: 1
source: jiuzhang, 算初DFS

Given an integer array `nums` that may contain duplicates, return *all possible subsets (the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]

```

**Example 2:**

```
Input: nums = [0]
Output: [[],[0]]

```

**Constraints:**

- `1 <= nums.length <= 10`
- `10 <= nums[i] <= 10`

重复->排序
第一个直接用，以后的只有i-1写到tmpRes里面才可以写i （放了第一个才能放第二个）
记录位置instead of value

```cpp
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        sort(nums.begin(),nums.end());
        helper(nums, res, tmp, 0);
        return res;
    }
    
    void helper(vector<int>& nums, vector<vector<int>>& res, vector<int>& tmp, int start){
        res.push_back(tmp);
        
        for(int i=start; i<nums.size(); i++){
            if(i>start && nums[i-1]==nums[i]) continue;
            tmp.push_back(nums[i]);
            helper(nums, res, tmp, i+1);
            tmp.pop_back();
        }
    }
};
```