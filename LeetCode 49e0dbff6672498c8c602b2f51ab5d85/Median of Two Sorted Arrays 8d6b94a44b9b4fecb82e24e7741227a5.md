# Median of Two Sorted Arrays

#: 4
Difficult: Hard
Tags: BS_Idx, kth
link: https://leetcode.com/problems/median-of-two-sorted-arrays/
Priority: High
Created time: July 16, 2022 11:50 AM
Last edited time: October 14, 2023 1:24 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初LinkedList&Array, 算初Tree Divide&Conquer

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.

```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

```

**Constraints:**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `106 <= nums1[i], nums2[i] <= 106`

Solution: interative:

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m=nums1.size();
        int n=nums2.size();
        if((m+n)&1){//odd
            return findKth(nums1,nums2,(m+n+1)/2);
        }
        else{//even
            return (findKth(nums1,nums2,(m+n+1)/2)+findKth(nums1,nums2,(m+n+1)/2+1))*0.5;
        }
    }
    
    //must not pass by reference
    int findKth(vector<int> nums1, vector<int> nums2, int k){
        int k1,k2;
        while(true){
            int n1=nums1.size();
            int n2=nums2.size();
            //corner case
            if(n1==0) return nums2[k-1];
            if(n2==0) return nums1[k-1];
            if(k==1) return min(nums1[0],nums2[0]);
            
            //not need to be very accurate to divide
            if(n1<=n2) {k1 = min(k>>1,n1), k2=k-k1;}
            else {k2 = min(k>>1,n2), k1=k-k2;}
            
            if(nums1[k1-1] < nums2[k2-1]){
                nums1.erase(nums1.begin(), nums1.begin()+k1);
                k-=k1;
            }
            else if(nums1[k1-1] > nums2[k2-1]){
                nums2.erase(nums2.begin(), nums2.begin()+k2);
                k-=k2;
            }
            else{
                return nums1[k1-1];
            }
        }
    }
};
```

Solution: recursion, above is better

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n1=nums1.size(), n2=nums2.size();
        if((n1+n2)&1) return findKth(nums1,nums2,(n1+n2+1)/2);
        else return (findKth(nums1,nums2,(n1+n2+1)/2) + findKth(nums1,nums2,(n1+n2+2)/2)) *0.5;
    }

    double findKth(vector<int> nums1, vector<int> nums2, int k){//kth
        if(nums1.size()>nums2.size()) swap(nums1,nums2);
        int n1=nums1.size(), n2=nums2.size();
        if(n1==0) return nums2[k-1];
        if(n2==0) return nums1[k-1];
        if(k==1) return min(nums1.front(), nums2.front());

        int k1=min(n1,k/2), k2=k-k1;
        if(nums1[k1-1]<nums2[k2-1]){
            nums1.erase(nums1.begin() , nums1.begin()+k1);//improve with start end instead of triming vector
            return findKth(nums1,nums2,k-k1);
        }
        else if(nums1[k1-1]>nums2[k2-1]){
            nums2.erase(nums2.begin() , nums2.begin()+k2);//improve with start end instead of triming vector
            return findKth(nums1,nums2,k-k2);
        }
        else{return nums1[k1-1];}
    }
};

/*
if odd, (m+n+1)/2 th
if even, ((m+n+1)/2 th + (m+n+2)/2 th) /2

findKth
nums1 k1
nums2 k2
k1+k2=k

corner case
    if k1=0
    if k2=0
    if smaller one size = 1

    smaller size -> min k/2, or n1

*/
```