# Kth Smallest Product of Two Sorted Arrays

#: 2040
Difficult: Hard
Tags: 2pt_SameDir_diffSpeed, BS_Idx, BS_Val, BS_lower_upper_bound, kth
link: https://leetcode.com/problems/kth-smallest-product-of-two-sorted-arrays/
Priority: High
Status: More Solution, No Idea At First, toRevisit
Created time: August 22, 2022 1:16 AM
Last edited time: October 22, 2023 7:06 PM
practicedTimes: 1
source: jiuzhang, 算高TwoPointers&PQ

Given two **sorted 0-indexed** integer arrays `nums1` and `nums2` as well as an integer `k`
, return *the* `kth` *(**1-based**) smallest product of* `nums1[i] * nums2[j]` *where* `0 <= i < nums1.length` *and* `0 <= j < nums2.length`.

**Example 1:**

```
Input: nums1 = [2,5], nums2 = [3,4], k = 2
Output: 8
Explanation: The 2 smallest products are:
- nums1[0] * nums2[0] = 2 * 3 = 6
- nums1[0] * nums2[1] = 2 * 4 = 8
The 2nd smallest product is 8.

```

**Example 2:**

```
Input: nums1 = [-4,-2,0,3], nums2 = [2,4], k = 6
Output: 0
Explanation: The 6 smallest products are:
- nums1[0] * nums2[1] = (-4) * 4 = -16
- nums1[0] * nums2[0] = (-4) * 2 = -8
- nums1[1] * nums2[1] = (-2) * 4 = -8
- nums1[1] * nums2[0] = (-2) * 2 = -4
- nums1[2] * nums2[0] = 0 * 2 = 0
- nums1[2] * nums2[1] = 0 * 4 = 0
The 6th smallest product is 0.

```

**Example 3:**

```
Input: nums1 = [-2,-1,0,1,2], nums2 = [-3,-1,2,4,5], k = 3
Output: -6
Explanation: The 3 smallest products are:
- nums1[0] * nums2[4] = (-2) * 5 = -10
- nums1[0] * nums2[3] = (-2) * 4 = -8
- nums1[4] * nums2[0] = 2 * (-3) = -6
The 3rd smallest product is -6.

```

**Constraints:**

- `1 <= nums1.length, nums2.length <= 5 * 104`
- `105 <= nums1[i], nums2[j] <= 105`
- `1 <= k <= nums1.length * nums2.length`
- `nums1` and `nums2` are sorted.

Solution:

```jsx
class Solution {
public:
    long long kthSmallestProduct(vector<int>& nums1, vector<int>& nums2, long long k) {
        int n1=nums1.size(); if(n1==0) {return 0;}
        int n2=nums2.size(); if(n2==0) {return 0;}
        long long l=-1e10, r=1e10;
        while(l<r){
            long long m=l+(r-l)/2;//2.0 will time limit exceeded
            if(cntSmallEqual(nums1,nums2,m)<k) l=m+1;
            else r=m;
        }
        return l;
    }
    
    long long cntSmallEqual(vector<int>& nums1, vector<int>& nums2, long long t){
        int n1=nums1.size();
        int n2=nums2.size();
        long long cnt=0;
        for(int i=0; i<n1; i++){
            long long a1=nums1[i];
						//a*b <= t
						//if a>0, b<= t/a
            if(a1>0){
                cnt+=(upper_bound(nums2.begin(),nums2.end(),t*1.0/a1)-nums2.begin());
            }
						//if a<0, b>= t/a
            else if(a1<0){
                cnt+=(nums2.end()-lower_bound(nums2.begin(),nums2.end(),t*1.0/a1));
            }
						//if a==0, 
            else{
                if(t>=0) cnt+=n2;
                else cnt+=0;
            }
        }
        return cnt;
    }
};
```