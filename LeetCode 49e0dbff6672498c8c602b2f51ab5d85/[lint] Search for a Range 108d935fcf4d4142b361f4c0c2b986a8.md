# [lint] Search for a Range

#: 61
Difficult: Medium
Tags: BS_Idx
link: https://www.lintcode.com/problem/61/
Priority: Pass
Created time: June 26, 2022 11:00 AM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算初BS

**Description**

Given a sorted array of `n` integers, find the starting and ending position of a given `target` value.

If the target is not found in the array, return `[-1, -1]`.

*Contact me on wechat to get **Amazon、Google** requent Interview questions . (wechat id : **jiuzhang0607**)*

**Example**

**Example 1:**

Input:

```
array = []
target = 9

```

Output:

```
[-1,-1]

```

Explanation:

9 is not in the array.

**Example 2:**

Input:

```
array = [5, 7, 7, 8, 8, 10]
target = 8

```

Output:

```
[3,4]

```

Explanation:

The [3,4] subinterval of the array 1 has the value 8.

**Challenge**

O(log *n*) time.

```cpp
class Solution {
public:
    /*
     * @param A: an integer sorted array
     * @param target: an integer to be inserted
     * @return: a list of length 2, [index1, index2]
     */
    vector<int> searchRange(vector<int> &A, int target) {
        vector<int> res(2,-1);
        int n=A.size(); if(n==0) return res;
        int left,right;
        //l
        int l=0,r=n-1;
        while(l<=r){
            int m=l+(r-l)/2;
            if(A[m]<target) l=m+1;
            else r=m-1;
        }
        if(l==n) return res;
        if(A[l]!=target) return res;
        else left=l;
        //r
        l=0,r=n-1;
        while(l<=r){
            int m=l+(r-l)/2;
            if(A[m]<=target) l=m+1;
            else r=m-1;
        }
        if(r==-1) return res;
        if(A[r]!=target) return res;
        else right=r;
        
        res={left,right};
        return res;
    }
};
```