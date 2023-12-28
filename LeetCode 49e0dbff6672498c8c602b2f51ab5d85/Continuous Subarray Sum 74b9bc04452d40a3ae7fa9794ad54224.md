# Continuous Subarray Sum

#: 523
Difficult: Medium
Tags: Array, Hash, Math, Prefix
link: https://leetcode.com/problems/continuous-subarray-sum/description/
Priority: High
Status: No Idea At First
Created time: December 3, 2023 1:15 AM
Last edited time: December 3, 2023 1:29 AM
source: facebookFreq

Given an integer array nums and an integer k, return `true` *if* `nums` *has a **good subarray** or* `false` *otherwise*.

A **good subarray** is a subarray where:

- its length is **at least two**, and
- the sum of the elements of the subarray is a multiple of `k`.

**Note** that:

- A **subarray** is a contiguous part of the array.
- An integer `x` is a multiple of `k` if there exists an integer `n` such that `x = n * k`. `0` is **always** a multiple of `k`.

**Example 1:**

```
Input: nums = [23,2,4,6,7], k = 6
Output: true
Explanation: [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.

```

**Example 2:**

```
Input: nums = [23,2,6,4,7], k = 6
Output: true
Explanation: [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.

```

**Example 3:**

```
Input: nums = [23,2,6,4,7], k = 13
Output: false

```

**Constraints:**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`
- `0 <= sum(nums[i]) <= 231 - 1`
- `1 <= k <= 231 - 1`

Solution prefix hash checking if prefix%k seen or not:

```cpp
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        int n=nums.size();// if(n==0) {return 0;}
        for(auto& x: nums) x%=k;
        unordered_map<int,int> mp;//prefixSum - val-firstSeenIdx
        mp[0]=-1;
        int sum=0;
        for(int i=0; i<n; i++){
            sum+=nums[i];
            //if exist and >= 2 elems
            if(mp.count(sum%k) && mp[sum%k]<i-1) return true;
            //add if not exist
            if(!mp.count(sum%k)) mp[sum%k]=i;
        }
        return false;
    }
};

/*
nums:
    len: 1~1e5
    val: 0~1e9
    sum: 0~INT_MAX
k: 1~INT_MAX
================================
return if nums has a substr that:
    len>=2
    sum%k=0;
================================
from editorial
    prefixSum[l-1]%k=prefixSum[r]%k
*/
```

Solution wrong:

e.g. 

nums -- 2 2 4 6 6

k = 7

we need to find subarr with sum = 14 but not 7

```cpp
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        int n=nums.size();// if(n==0) {return 0;}
        for(auto& x: nums) x%=k;
        //check case 0
        for(int i=1; i<n; i++){
            if(nums[i]==0 && nums[i-1]==0) return true;
        }
        std::cout<< "nums" <<" -- "; for (const auto& elem : nums) { cout << setw(4) << elem << " "; } std::cout<<std::endl;
        //check case k
        int l=0, r=0;
        int sum=nums[0];
        while(l<n && r<n){
            //if sum small, r++
            if(sum<k){
                r++;
                sum+=nums[r];
            }
            //if sum large, l++
            else if(sum>k){
                sum-=nums[l];
                l++;
            }
            else return true;
        }
        return false;
    }
};

/*
nums:
    len: 1~1e5
    val: 0~1e9
    sum: 0~INT_MAX
k: 1~INT_MAX
================================
return if nums has a substr that:
    len>=2
    sum%k=0;
================================
    check len>=2
    for each num, num%=k
    find a str that sum==k or 0
        case k: sliding win
        case 0: if continuous 2 elems both is 0
*/
```