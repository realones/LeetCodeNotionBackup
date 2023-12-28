# Contains Duplicate II

#: 219
Difficult: Easy
Tags: 2pt_SameDir_sameSpeed, Array, Hash, Sliding Window
link: https://leetcode.com/problems/contains-duplicate-ii/
Priority: Medium
Created time: June 2, 2023 10:59 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given an integer array `nums` and an integer `k`, return `true` *if there are two **distinct indices*** `i` *and* `j` *in the array such that* `nums[i] == nums[j]` *and* `abs(i - j) <= k`.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3
Output: true

```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1
Output: true

```

**Example 3:**

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false

```

**Constraints:**

- `1 <= nums.length <= 105`
- `109 <= nums[i] <= 109`
- `0 <= k <= 105`

Solution - hash:

```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n=nums.size();
        unordered_map<int,int> mp;//val_idx
        for(int i=0; i<n; i++){
            if(mp.count(nums[i]) && abs(mp[nums[i]] - i)<=k){
                return true;
            }
            mp[nums[i]]=i;
        }
        return false;
    }
};
```

Solution - 2pt same speed

```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        if(k<1 || nums.size()<=1) return false;
        unordered_set<int> s;
        for(int i=0;i<nums.size();i++){
            if(i>k){s.erase(nums[i-k-1]);}
            if(s.count(nums[i])>0) return true;
            else{s.insert(nums[i]);}
        }
        return false;
    }
};
```