# Count Number of Pairs With Absolute Difference K

#: 2006
Difficult: Easy
Tags: 2pt_SameDir_diffSpeed, Array, Hash
link: https://leetcode.com/problems/count-number-of-pairs-with-absolute-difference-k/description/
Priority: Medium
Created time: October 13, 2023 4:38 PM
Last edited time: October 13, 2023 6:24 PM
source: leetcode_daily
related: [lint] Two Sum - Difference equals to target (%5Blint%5D%20Two%20Sum%20-%20Difference%20equals%20to%20target%2099c7f8d7fefa45ad917270ad5b68dc0a.md)

Given an integer array `nums` and an integer `k`, return *the number of pairs* `(i, j)` *where* `i < j` *such that* `|nums[i] - nums[j]| == k`.

The value of `|x|` is defined as:

- `x` if `x >= 0`.
- `x` if `x < 0`.

**Example 1:**

```
Input: nums = [1,2,2,1], k = 1
Output: 4
Explanation: The pairs with an absolute difference of 1 are:
- [1,2,2,1]
- [1,2,2,1]
- [1,2,2,1]
- [1,2,2,1]

```

**Example 2:**

```
Input: nums = [1,3], k = 3
Output: 0
Explanation: There are no pairs with an absolute difference of 3.

```

**Example 3:**

```
Input: nums = [3,2,1,5,4], k = 2
Output: 3
Explanation: The pairs with an absolute difference of 2 are:
- [3,2,1,5,4]
- [3,2,1,5,4]
- [3,2,1,5,4]

```

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`
- `1 <= k <= 99`

Solution 2pt:

```cpp
class Solution {
public:
    int countKDifference(vector<int>& nums, int k) {
        int cnt=0;
        int n=nums.size();
        sort(nums.begin(), nums.end());
        for(int l=0,r=0; r<n; ){
            int diff=nums[r]-nums[l];
            if(diff<k) r++;
            else if(diff>k) l++;
            else{
                int lCnt=1; 
                for(l++; l < n && nums[l] == nums[l-1]; l++) lCnt++;
                int rCnt=1;
                for(r++; r < n && nums[r] == nums[r-1]; r++) rCnt++;
                cnt+=lCnt*rCnt;
            }
        }
        return cnt;
    }
};
```

Solution hash:

```cpp
class Solution {
public:
    int countKDifference(vector<int>& nums, int k) {
        unordered_map<int,int> mp;//val,cnt
        int res=0;
        for(auto x: nums){
            if(mp.count(x-k)) res+=mp[x-k];
            if(mp.count(x+k)) res+=mp[x+k];
            mp[x]++;
        }
        return res;
    }
}
```