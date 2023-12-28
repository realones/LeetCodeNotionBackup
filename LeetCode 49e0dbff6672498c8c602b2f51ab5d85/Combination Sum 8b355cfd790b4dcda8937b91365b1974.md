# Combination Sum

#: 39
Difficult: Medium
Tags: BackTracking, Combination, search
link: https://leetcode.com/problems/combination-sum/
Priority: Low
Created time: July 28, 2022 12:37 AM
Last edited time: October 16, 2023 4:49 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算初DFS

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]

```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []

```

**Constraints:**

- `1 <= candidates.length <= 30`
- `1 <= candidates[i] <= 200`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 500`

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& nums, int target) {
        vector<vector<int>> subs;
        vector<int> sub;
        helper(nums,subs,sub,0,target);
        return subs;
    }

private:    
    void helper(vector<int>& nums, vector<vector<int>>& subs, vector<int>& sub, int start, int target){
        if(target==0){
            subs.push_back(sub);
            return;
        }
        if(target<0) return;
        
        for(int i=start; i<nums.size(); i++){
            sub.push_back(nums[i]);
            helper(nums,subs,sub,i,target-nums[i]);
            sub.pop_back();
        }
    }
};
```