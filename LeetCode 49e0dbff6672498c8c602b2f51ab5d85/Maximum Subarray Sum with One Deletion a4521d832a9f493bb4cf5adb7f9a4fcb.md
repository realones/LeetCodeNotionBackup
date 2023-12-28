# Maximum Subarray Sum with One Deletion

#: 1186
Difficult: Medium
Tags: DP, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/maximum-subarray-sum-with-one-deletion/
Priority: Medium
Created time: November 19, 2022 11:24 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

Given an array of integers, return the maximum sum for a **non-empty** subarray (contiguous elements) with at most one element deletion. In other words, you want to choose a subarray and optionally delete one element from it so that there is still at least one element left and the sum of the remaining elements is maximum possible.

Note that the subarray needs to be **non-empty** after deleting one element.

**Example 1:**

```
Input: arr = [1,-2,0,3]
Output: 4
Explanation:Because we can choose [1, -2, 0, 3] and drop -2, thus the subarray [1, 0, 3] becomes the maximum value.
```

**Example 2:**

```
Input: arr = [1,-2,-2,3]
Output: 3
Explanation:We just choose [3] and it's the maximum sum.

```

**Example 3:**

```
Input: arr = [-1,-1,-1,-1]
Output: -1
Explanation: The final subarray needs to be non-empty. You can't choose [-1] and delete -1 from it, then get an empty subarray to make the sum equals to 0.

```

**Constraints:**

- `1 <= arr.length <= 105`
- `104 <= arr[i] <= 104`

```cpp
class Solution {
public:
    int maximumSum(vector<int>& nums) {
        int n=nums.size();
        int notDeleted=nums[0], deleted=INT_MIN;
        int res=nums[0];
        
        for(int i=1;i<n;i++){
            deleted = max((deleted<0?0:deleted)+nums[i]//no op
                ,notDeleted);//delete
            notDeleted = (notDeleted<0?0:notDeleted)+nums[i];//no op
            
            res=max({res, deleted, notDeleted});
        }
        
        return res;
    }
};
```