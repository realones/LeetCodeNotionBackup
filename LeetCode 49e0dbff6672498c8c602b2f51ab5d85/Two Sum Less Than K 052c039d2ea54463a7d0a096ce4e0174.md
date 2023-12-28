# Two Sum Less Than K

#: 1099
Difficult: Easy
Tags: 2pt, Array, BS_todo, Sort
link: https://leetcode.com/problems/two-sum-less-than-k/
Priority: Low
Created time: November 30, 2023 7:16 PM
Last edited time: November 30, 2023 7:17 PM
source: leetcode_daily

Given an array `nums` of integers and integer `k`, return the maximum `sum` such that there exists `i < j` with `nums[i] + nums[j] = sum` and `sum < k`. If no `i`, `j` exist satisfying this equation, return `-1`.

**Example 1:**

```
Input: nums = [34,23,1,24,75,33,54,8], k = 60
Output: 58
Explanation:We can use 34 and 24 to sum 58 which is less than 60.

```

**Example 2:**

```
Input: nums = [10,20,30], k = 15
Output: -1
Explanation:In this case it is not possible to get a pair sum less that 15.

```

**Constraints:**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 1000`
- `1 <= k <= 2000`

```cpp
class Solution {
public:
    int twoSumLessThanK(vector<int>& nums, int k) {
        int n=nums.size();// if(n==0) {return 0;}
        sort(nums.begin(), nums.end());
        int res=-1;
        for(int l=0,r=n-1; l<r; ){
            int tmp=nums[l]+nums[r];
            if(tmp>=k) r--;
            else{
                res=max(res,tmp);
                l++;
            } 
        }
        return res;
    }
};

/*
2pt
sort
l:0->. r:<-n-1
if too big, r--
if too small, l++
*/
```