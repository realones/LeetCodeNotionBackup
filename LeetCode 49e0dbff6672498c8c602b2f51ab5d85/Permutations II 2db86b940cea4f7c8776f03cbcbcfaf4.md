# Permutations II

#: 47
Difficult: Medium
Tags: BackTracking, Permutation
link: https://leetcode.com/problems/permutations-ii/
Priority: Low
Created time: July 29, 2022 3:37 PM
Last edited time: October 16, 2023 4:51 PM
practicedTimes: 1
source: jiuzhang, 算初DFS

Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.*

**Example 1:**

```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]

```

**Example 2:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

```

**Constraints:**

- `1 <= nums.length <= 8`
- `10 <= nums[i] <= 10`

```cpp
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        vector<bool> visited(nums.size(),false);
        sort(nums.begin(),nums.end());
        helper(nums,res,tmp,visited);
        return res;
    }
    
    void helper(vector<int>& nums, vector<vector<int>>& res, vector<int>& tmp, vector<bool>& visited){
        if(tmp.size()==nums.size()){
            res.push_back(tmp);
            return;
        }
            
        for(int i=0; i<nums.size(); i++){
            if(visited[i]) continue;
            if(i>0 && nums[i]==nums[i-1] && !visited[i-1]) continue;
            tmp.push_back(nums[i]); visited[i]=true;
            helper(nums,res,tmp,visited);
            tmp.pop_back(); visited[i]=false;;
        }
    }
};
```