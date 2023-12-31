# Make K-Subarray Sums Equal

#: 2607
Difficult: Medium
Tags: Array, Math, QuickSelect, Sort, kth
link: https://leetcode.com/problems/make-k-subarray-sums-equal/description/
Priority: High
Status: Dig More, No Idea At First
Created time: November 25, 2023 9:36 PM
Last edited time: November 25, 2023 9:37 PM
source: AmazonFreq

You are given a **0-indexed** integer array `arr` and an integer `k`. The array `arr` is circular. In other words, the first element of the array is the next element of the last element, and the last element of the array is the previous element of the first element.

You can do the following operation any number of times:

- Pick any element from `arr` and increase or decrease it by `1`.

Return *the minimum number of operations such that the sum of each **subarray** of length* `k` *is equal*.

A **subarray** is a contiguous part of the array.

**Example 1:**

```
Input: arr = [1,4,1,3], k = 2
Output: 1
Explanation: we can do one operation on index 1 to make its value equal to 3.
The array after the operation is [1,3,1,3]
- Subarray starts at index 0 is [1, 3], and its sum is 4
- Subarray starts at index 1 is [3, 1], and its sum is 4
- Subarray starts at index 2 is [1, 3], and its sum is 4
- Subarray starts at index 3 is [3, 1], and its sum is 4

```

**Example 2:**

```
Input: arr = [2,5,5,7], k = 3
Output: 5
Explanation: we can do three operations on index 0 to make its value equal to 5 and two operations on index 3 to make its value equal to 5.
The array after the operations is [5,5,5,5]
- Subarray starts at index 0 is [5, 5, 5], and its sum is 15
- Subarray starts at index 1 is [5, 5, 5], and its sum is 15
- Subarray starts at index 2 is [5, 5, 5], and its sum is 15
- Subarray starts at index 3 is [5, 5, 5], and its sum is 15

```

**Constraints:**

- `1 <= k <= arr.length <= 105`
- `1 <= arr[i] <= 109`

```cpp
using ll=long long;

class Solution {
public:
    long long makeSubKSumEqual(vector<int>& nums, int k) {
        int n=nums.size();
        ll res=0;
        for(int i=0;i<k;i++){
            vector<int> tmp;
            for(int j=i;nums[j]!=0;j=(j+k)%n){
                tmp.push_back(nums[j]);
                nums[j]=0;
            }
            nth_element(tmp.begin(), tmp.begin()+tmp.size()/2, tmp.end());
            for(auto x: tmp){
                res+= abs(x-tmp[tmp.size()/2]);
            }
        }
        return res;
    }
};

/*
nums:
    len: 1~1e5
    val: 1~1e9
k: 1~n
================================
solution votrubac:
sum of all subarr.sz==k is equal if
arr[i]==arr[i+x*k], x=1,2,3,..
================================
*/
```