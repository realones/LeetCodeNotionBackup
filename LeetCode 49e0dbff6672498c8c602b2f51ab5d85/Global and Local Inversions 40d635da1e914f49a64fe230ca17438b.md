# Global and Local Inversions

#: 775
Difficult: Medium
Tags: Array, Divide and Conquer, Math, MergeSort
link: https://leetcode.com/problems/global-and-local-inversions/
Priority: Medium
Status: More Solution, dup, toRevisit
Created time: June 21, 2023 8:21 PM
Last edited time: October 17, 2023 6:24 PM
source: HuaHua

You are given an integer array `nums` of length `n` which represents a permutation of all the integers in the range `[0, n - 1]`.

The number of **global inversions** is the number of the different pairs `(i, j)` where:

- `0 <= i < j < n`
- `nums[i] > nums[j]`

The number of **local inversions** is the number of indices `i` where:

- `0 <= i < n - 1`
- `nums[i] > nums[i + 1]`

Return `true` *if the number of **global inversions** is equal to the number of **local inversions***.

**Example 1:**

```
Input: nums = [1,0,2]
Output: true
Explanation: There is 1 global inversion and 1 local inversion.

```

**Example 2:**

```
Input: nums = [1,2,0]
Output: false
Explanation: There are 2 global inversions and 1 local inversion.

```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 105`
- `0 <= nums[i] < n`
- All the integers of `nums` are **unique**.
- `nums` is a permutation of all the numbers in the range `[0, n - 1]`.

todo: find another solution

Solution 1: merge sort:

```cpp
using ll=long long;
class Solution {
public:
    bool isIdealPermutation(vector<int>& nums) {
        int n=nums.size();
        ll local=0;
        for(int i=0; i<n-1; i++){
            if(nums[i]>nums[i+1]) local++;
        }
        vector<int> tmp=nums;
        ll global=mergeSort(nums, tmp, 0, n-1);
        return local==global;
    }

    ll mergeSort(vector<int>& nums, vector<int>& tmp, int start, int end){
        if(start>=end) return 0;
        int mid=start+(end-start)/2;
        ll l=mergeSort(nums, tmp, start, mid);
        ll r=mergeSort(nums, tmp, mid+1, end);
        ll m=merge(nums, tmp, start, end);
        return l+r+m;
    }

    ll merge(vector<int>& nums, vector<int>& tmp, int start ,int end){
        ll res=0;
        int mid=start+(end-start)/2;
        int l=start, r=mid+1, i=start;
        while(l<=mid && r<=end){
            if(nums[l]<nums[r]) tmp[i++]=nums[l++];
            else {tmp[i++]=nums[r++]; res+=mid-l+1;}
        }

        while(l<=mid) tmp[i++]=nums[l++];
        while(r<=end) tmp[i++]=nums[r++];

        for(int i=start; i<=end; i++){
            nums[i]=tmp[i];
        }

        return res;
    }
};
```