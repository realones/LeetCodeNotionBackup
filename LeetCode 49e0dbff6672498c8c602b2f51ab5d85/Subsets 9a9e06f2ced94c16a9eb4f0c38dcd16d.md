# Subsets

#: 78
Difficult: Medium
Tags: BackTracking, BitMask, Combination, search
link: https://leetcode.com/problems/subsets
Priority: Low
Status: More Solution
Created time: July 28, 2022 12:37 AM
Last edited time: October 16, 2023 4:47 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初DFS

Given an integer array `nums` of **unique** elements, return *all possible subsets (the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

```

**Example 2:**

```
Input: nums = [0]
Output: [[],[0]]

```

**Constraints:**

- `1 <= nums.length <= 10`
- `10 <= nums[i] <= 10`
- All the numbers of `nums` are **unique**.

3 solutions (backtracing, iterative, bit_manipulation)

back tracing, still can use DP, go dp solution

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> subs;
        vector<int> sub;
        helper(nums,subs,sub,0);
        return subs;
    }
    
    void helper(vector<int>& nums, vector<vector<int>>& subs, vector<int>& sub, int start){
        subs.push_back(sub);
        for(int i=start; i<nums.size(); i++){
            sub.push_back(nums[i]);
            helper(nums,subs,sub,i+1);
            sub.pop_back();
        }
    }
};
```