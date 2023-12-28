# Maximum Score of a Good Subarra

#: 1793
Difficult: Hard
Tags: 2pt, Array, BS_Idx, Greedy, monotonic, stack
link: https://leetcode.com/problems/maximum-score-of-a-good-subarray/
Priority: High
Status: More Solution, No Idea At First, toRevisit
Created time: May 12, 2023 2:43 AM
Last edited time: October 22, 2023 5:40 PM
source: leetcode_daily, wisdompeak
related: Largest Rectangle in Histogram (Largest%20Rectangle%20in%20Histogram%20ac2ea76b0a0645e1b28ad0283e4f7fee.md)

You are given an array of integers `nums` **(0-indexed)** and an integer `k`.

The **score** of a subarray `(i, j)` is defined as `min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)`. A **good** subarray is a subarray where `i <= k <= j`.

Return *the maximum possible **score** of a **good** subarray.*

**Example 1:**

```
Input: nums = [1,4,3,7,4,5], k = 3
Output: 15
Explanation: The optimal subarray is (1, 5) with a score of min(4,3,7,4,5) * (5-1+1) = 3 * 5 = 15.

```

**Example 2:**

```
Input: nums = [5,5,4,5,4,1,1,1], k = 0
Output: 20
Explanation: The optimal subarray is (0, 4) with a score of min(5,5,4,5,4) * (4-0+1) = 4 * 5 = 20.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 2 * 104`
- `0 <= k < nums.length`

todo: check other 2 solutions in Editorial, check lee215

Solution bs_idx:

```cpp
class Solution {
public:
    int maximumScore(vector<int>& nums, int k) {
        int cand1=helper(nums,k);
        reverse(nums.begin(), nums.end());
        int cand2=helper(nums,nums.size()-1-k);
        return max(cand1,cand2);
    }
    int helper(vector<int>& nums, int k) {
        int n=nums.size();
        //preprocess the min val for [i~k] and [k~j]
        vector<int> lmin(n,nums[k]), rmin(n,nums[k]);
        for(int i=k-1;i>=0;i--){
            lmin[i] = min(lmin[i+1],nums[i]);
        }
        for(int i=k+1;i<n;i++){
            rmin[i] = min(rmin[i-1],nums[i]);
        }
        //loop right side elems, find best left elem
        int res=0;
        for(int r=k; r<n; r++){
            int min=rmin[r];
            int l=lower_bound(lmin.begin(), lmin.begin()+k, min) - lmin.begin();
            res=max(res, min*(r-l+1));
        }
        return res;
    }
};

/*
nums:
    len: 1~1e5
    val: 1~2e4
k: 0~n-1, always valid
================================
subarray:
    min_val * len
    good: i<=k<=j
return: max score of good subarr
================================
preprocess to get lmin and rmin to O(1) get min of subarr - O(n)

TLE: left side O(n) possibilities * right side O(n) possiblities --> O(n^2)
TO IMPROVE: loop one side and find best of other side
    loop right side O(n) possbilities
        find best left side selection O(logn) BS
            since left side min val is from small to large sorted, 
            find the first one that >= right side min
        update res
    loop left side ... (similar with above)
TO IMPROVE: only loop right side, then reverse the nums, and do it again
================================
T: O(nlogn)
S: O(n)
*/
```

Solution - Mono Stk

Solution - 2pt - Greedy

```cpp
class Solution {
public:
    int maximumScore(vector<int>& nums, int k) {
        int n=nums.size();
        //preprocess the min val for [i~k] and [k~j]
        vector<int> lmin(n,nums[k]), rmin(n,nums[k]);
        for(int i=k-1;i>=0;i--){
            lmin[i] = min(lmin[i+1],nums[i]);
        }
        for(int i=k+1;i<n;i++){
            rmin[i] = min(rmin[i-1],nums[i]);
        }
        //2pt diff direction
        //-- len mono dec, min mono inc
        int res=0;
        for(int l=0,r=n-1; l<=k && k<=r;){
            int len=r-l+1;
            int minv=min(lmin[l],rmin[r]);
            res=max(res, len*minv);
            if(l<k && lmin[l]<=rmin[r]) l++;
            else if(r>k && lmin[l]>=rmin[r]) r--;
            else break;//cut branch
        }
        return res;
    }
};
```