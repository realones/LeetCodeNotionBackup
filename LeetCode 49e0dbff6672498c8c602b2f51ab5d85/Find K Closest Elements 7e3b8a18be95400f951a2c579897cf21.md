# Find K Closest Elements

#: 658
Difficult: Medium
Tags: Array, BS_Idx
link: https://leetcode.com/problems/find-k-closest-elements/description/
Priority: Medium
Status: HardToImplement, More Solution, toRevisit
Created time: October 11, 2023 11:40 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

Given a **sorted** integer array `arr`, two integers `k` and `x`, return the `k` closest integers to `x` in the array. The result should also be sorted in ascending order.

An integer `a` is closer to `x` than an integer `b` if:

- `|a - x| < |b - x|`, or
- `|a - x| == |b - x|` and `a < b`

**Example 1:**

```
Input: arr = [1,2,3,4,5], k = 4, x = 3
Output: [1,2,3,4]

```

**Example 2:**

```
Input: arr = [1,2,3,4,5], k = 4, x = -1
Output: [1,2,3,4]

```

**Constraints:**

- `1 <= k <= arr.length`
- `1 <= arr.length <= 104`
- `arr` is sorted in **ascending** order.
- `104 <= arr[i], x <= 104`

```cpp
class Solution {
public:
    vector<int> findClosestElements(vector<int>& nums, int k, int x) {
        int n=nums.size();
        vector<int> res;
        int idx = lower_bound(nums.begin(), nums.end(), x) - nums.begin();
        if(idx==n) idx=n-1;
        int l=idx-1, r=idx;
        for(int i=0; i<k; i++){//k times
            if(r>n-1 || l>=0 && abs(nums[l]-x) <= abs(nums[r]-x)){
                res.push_back(nums[l]); l--;
            }
            else{
                res.push_back(nums[r]); r++;
            }
        }
        sort(res.begin(), res.end());
        return res;
    }
};
```