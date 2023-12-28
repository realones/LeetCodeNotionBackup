# Combination Sum II

#: 40
Difficult: Medium
Tags: BackTracking, Combination, search
link: https://leetcode.com/problems/combination-sum-ii/
Priority: Low
Created time: July 28, 2022 12:37 AM
Last edited time: October 16, 2023 4:49 PM
practicedTimes: 1
source: jiuzhang, 算初DFS

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5
Output:
[
[1,2,2],
[5]
]

```

**Constraints:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& nums, int target) {
        vector<vector<int>> subs;
        vector<int> sub;
        sort(nums.begin(),nums.end());
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
            if(i>start && nums[i]==nums[i-1]) continue;
            sub.push_back(nums[i]);
            helper(nums,subs,sub,i+1,target-nums[i]);
            sub.pop_back();
        }
    }
};
```