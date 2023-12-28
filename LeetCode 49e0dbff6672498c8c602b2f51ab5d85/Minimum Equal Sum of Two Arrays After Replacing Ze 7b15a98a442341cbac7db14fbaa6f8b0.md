# Minimum Equal Sum of Two Arrays After Replacing Zeros

#: 2918
Difficult: Medium
Tags: Array
link: https://leetcode.com/problems/minimum-equal-sum-of-two-arrays-after-replacing-zeros/description/
Priority: Low
Created time: October 29, 2023 12:32 AM
Last edited time: October 29, 2023 12:32 AM
source: LC_Contests

You are given two arrays `nums1` and `nums2` consisting of positive integers.

You have to replace **all** the `0`'s in both arrays with **strictly** positive integers such that the sum of elements of both arrays becomes **equal**.

Return *the **minimum** equal sum you can obtain, or* `-1` *if it is impossible*.

**Example 1:**

```
Input: nums1 = [3,2,0,1,0], nums2 = [6,5,0]
Output: 12
Explanation: We can replace 0's in the following way:
- Replace the two 0's in nums1 with the values 2 and 4. The resulting array is nums1 = [3,2,2,1,4].
- Replace the 0 in nums2 with the value 1. The resulting array is nums2 = [6,5,1].
Both arrays have an equal sum of 12. It can be shown that it is the minimum sum we can obtain.

```

**Example 2:**

```
Input: nums1 = [2,0,2,0], nums2 = [1,4]
Output: -1
Explanation: It is impossible to make the sum of both arrays equal.

```

**Constraints:**

- `1 <= nums1.length, nums2.length <= 105`
- `0 <= nums1[i], nums2[i] <= 106`

```cpp
using ll=long long;

class Solution {
public:
    long long minSum(vector<int>& nums1, vector<int>& nums2) {//len: 1~1e5, val: 0~1e6
        int n1=nums1.size(), n2=nums2.size();

        ll sum1=0; int cnt1=0;
        for(auto x: nums1){
            if(x==0) sum1++, cnt1++;
            else sum1+=x;
        }

        ll sum2=0; int cnt2=0;
        for(auto x: nums2){
            if(x==0) sum2++, cnt2++;
            else sum2+=x;
        }

        if(cnt1>0 && cnt2>0) return max(sum1,sum2);
        else if(cnt1==0 && cnt2==0) return sum1==sum2? sum1: -1;
        else if(cnt1==0){
            if(sum2>sum1) return -1;
            return sum1;
        }
        else{//cnt2==0
            if(sum1>sum2) return -1;
            return sum2;
        }
    }
};

/*
lsum, for each 0, calcu as 1
rsum, for each 0, calcu as 1

l0cnt 
r0cnt

if both side has 0s, then res=max(lsum, rsum)

if both side not has 0s, then if lsum==rsum, return lsum, else return -1

only if one side no 0, 
    the side with 0 should <= other side, otherwise -1

*/
```