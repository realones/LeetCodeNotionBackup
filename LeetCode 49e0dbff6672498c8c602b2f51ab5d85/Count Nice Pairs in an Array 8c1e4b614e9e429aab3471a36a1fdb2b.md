# Count Nice Pairs in an Array

#: 1814
Difficult: Medium
Tags: Array, Counting, Hash, Math
link: https://leetcode.com/problems/count-nice-pairs-in-an-array/
Priority: Medium
Status: No Idea At First
Created time: November 20, 2023 6:01 PM
Last edited time: November 20, 2023 6:02 PM
source: leetcode_daily

You are given an array `nums` that consists of non-negative integers. Let us define `rev(x)` as the reverse of the non-negative integer `x`. For example, `rev(123) = 321`, and `rev(120) = 21`. A pair of indices `(i, j)` is **nice** if it satisfies all of the following conditions:

- `0 <= i < j < nums.length`
- `nums[i] + rev(nums[j]) == nums[j] + rev(nums[i])`

Return *the number of nice pairs of indices*. Since that number can be too large, return it **modulo** `109 + 7`.

**Example 1:**

```
Input: nums = [42,11,1,97]
Output: 2
Explanation: The two pairs are:
 - (0,3) : 42 + rev(97) = 42 + 79 = 121, 97 + rev(42) = 97 + 24 = 121.
 - (1,2) : 11 + rev(1) = 11 + 1 = 12, 1 + rev(11) = 1 + 11 = 12.

```

**Example 2:**

```
Input: nums = [13,10,35,24,76]
Output: 4

```

**Constraints:**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int countNicePairs(vector<int>& nums) {
        int n=nums.size();
        for(auto& x: nums){
            x-=rev(x);
        }
        //get freq
        unordered_map<int,int> mp;//freq
        for(auto x: nums){
            mp[x]++;
        }
        //count
        ll res=0;
        for(auto [_,cnt]: mp){
            res+= (ll)cnt*(cnt-1)/2;
            res%=M;
        }
        return res;
    }

    int rev(int x){
        int res=0;
        while(x){
            res*=10;
            res+=x%10;
            x/=10;
        }
        return res;
    }
};

/*
from the hints
nums[j]-r(nums[j]) == nums[j]-r(nums[j])
*/
```