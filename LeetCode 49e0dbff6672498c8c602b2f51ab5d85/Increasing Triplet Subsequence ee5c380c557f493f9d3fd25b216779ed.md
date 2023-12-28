# Increasing Triplet Subsequence

#: 334
Difficult: Medium
Tags: Array, Greedy, Prefix
link: https://leetcode.com/problems/increasing-triplet-subsequence/
Priority: High
Status: More Solution, No Idea At First
Created time: June 27, 2023 11:16 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

Given an integer array `nums`, return `true` *if there exists a triple of indices* `(i, j, k)` *such that* `i < j < k` *and* `nums[i] < nums[j] < nums[k]`. If no such indices exists, return `false`.

**Example 1:**

```
Input: nums = [1,2,3,4,5]
Output: true
Explanation: Any triplet where i < j < k is valid.

```

**Example 2:**

```
Input: nums = [5,4,3,2,1]
Output: false
Explanation: No triplet exists.

```

**Example 3:**

```
Input: nums = [2,1,5,0,4,6]
Output: true
Explanation: The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.

```

**Constraints:**

- `1 <= nums.length <= 5 * 105`
- `231 <= nums[i] <= 231 - 1`

**Follow up:**

Could you implement a solution that runs in

```
O(n)
```

time complexity and

```
O(1)
```

space complexity?

```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        if(nums.empty()) return false;
        
        vector<bool> v(nums.size(),false);
        int curMin=INT_MAX, curMax=INT_MIN;
        for(int i=0;i<nums.size();i++){
            if(nums[i]<=curMin){curMin=nums[i]; v[i]=false;}
            else{v[i]=true;}//if increasing
        }
        for(int i=nums.size()-1;i>=0;i--){
            if(nums[i]>=curMax){curMax=nums[i]; v[i]=false;}
            else{
                if(v[i]==true) return true;
            }//if decreasing
        }
        return false;
    }
};
```