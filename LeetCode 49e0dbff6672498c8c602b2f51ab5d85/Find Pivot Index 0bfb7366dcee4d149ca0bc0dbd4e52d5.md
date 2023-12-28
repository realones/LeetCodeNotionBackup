# Find Pivot Index

#: 724
Difficult: Easy
Tags: DP, Prefix
link: https://leetcode.com/problems/find-pivot-index
Priority: Pass
Created time: June 28, 2023 12:53 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

Given an array of integers `nums`, calculate the **pivot index** of this array.

The **pivot index** is the index where the sum of all the numbers **strictly** to the left of the index is equal to the sum of all the numbers **strictly** to the index's right.

If the index is on the left edge of the array, then the left sum is `0` because there are no elements to the left. This also applies to the right edge of the array.

Return *the **leftmost pivot index***. If no such index exists, return `-1`.

**Example 1:**

```
Input: nums = [1,7,3,6,5,6]
Output: 3
Explanation:
The pivot index is 3.
Left sum = nums[0] + nums[1] + nums[2] = 1 + 7 + 3 = 11
Right sum = nums[4] + nums[5] = 5 + 6 = 11

```

**Example 2:**

```
Input: nums = [1,2,3]
Output: -1
Explanation:
There is no index that satisfies the conditions in the problem statement.
```

**Example 3:**

```
Input: nums = [2,1,-1]
Output: 0
Explanation:
The pivot index is 0.
Left sum = 0 (no elements to the left of index 0)
Right sum = nums[1] + nums[2] = 1 + -1 = 0

```

**Constraints:**

- `1 <= nums.length <= 104`
- `1000 <= nums[i] <= 1000`

```cpp
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int n=nums.size();
        vector<int> prefixSum(nums);
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }

        for(int i=0; i<n; i++){
            if(getPrefixSum(prefixSum,0,i-1) == getPrefixSum(prefixSum, i+1,n-1)){
                return i;
            }
        }
        return -1;
    }

    int getPrefixSum(vector<int>& prefixSum, int l, int r){
        if(r<0 || l>prefixSum.size()-1) return 0;
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```