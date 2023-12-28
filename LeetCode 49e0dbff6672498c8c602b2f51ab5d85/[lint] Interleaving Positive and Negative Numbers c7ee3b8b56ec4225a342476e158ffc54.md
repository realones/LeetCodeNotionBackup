# [lint] Interleaving Positive and Negative Numbers

#: 144
Difficult: Medium
Tags: Partition
link: https://www.lintcode.com/problem/interleaving-positive-and-negative-numbers
Priority: Low
Created time: August 9, 2022 11:31 AM
Last edited time: October 19, 2023 10:05 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers
related: Rearrange Array Elements by Sign (Rearrange%20Array%20Elements%20by%20Sign%2005343857e43440fdbb6a9787cfd5e49d.md)

**Description**

Given an array with positive and negative integers. Re-range it to interleaving with positive and negative integers.

# 

You are not necessary to keep the original order of positive integers or negative integers.

**Example**

***Example 1***

```
Input : [-1, -2, -3, 4, 5, 6]
Outout : [-1, 5, -2, 4, -3, 6]
Explanation :  any other reasonable answer.

```

**Challenge**

Do it in-place and without extra memory.

better solution with same direct two pointers? seems not possible 

```cpp
class Solution {
public:
    /*
     * @param A: An integer array.
     * @return: nothing
     */
    void rerange(vector<int> &A) {
        // write your code here
        if(A.size()<=1) return;
        patition(A);
        interleaving(A);
    }
    // ---------+++++++++++++
    //return num of ----
    void patition(vector<int> &A){
        int left=0,right=A.size()-1;
        while(left<=right){
            while(left<=right && A[left]<0) left++;
            while(left<=right && A[right]>0) right--;
            if(left<=right){
                swap(A[left],A[right]);
                left++;right--;
            }
        }
        return;
    }
    
    void interleaving(vector<int> &A){
        int pos=0,neg=0;
        for(auto a:A){
            if(a>0) pos++;
            else neg++;
        }
        int left=0,right=A.size()-1;
        if(neg>pos){left++;}
        else if(neg<pos){right--;}
        
        for(;left<right;left+=2,right-=2){
            swap(A[left],A[right]);
        }
        
        return;
    }
};
```