# Peak Index in a Mountain Array

#: 852
Difficult: Medium
Tags: BS_Idx
link: https://leetcode.com/problems/peak-index-in-a-mountain-array/description/
Priority: Pass
Created time: July 25, 2023 11:13 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

An array `arr` a **mountain** if the following properties hold:

- `arr.length >= 3`
- There exists some `i` with `0 < i < arr.length - 1` such that:
    - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given a mountain array `arr`, return the index `i` such that `arr[0] < arr[1] < ... < arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`.

You must solve it in `O(log(arr.length))` time complexity.

**Example 1:**

```
Input: arr = [0,1,0]
Output: 1

```

**Example 2:**

```
Input: arr = [0,2,1,0]
Output: 1

```

**Example 3:**

```
Input: arr = [0,10,5,2]
Output: 1

```

**Constraints:**

- `3 <= arr.length <= 105`
- `0 <= arr[i] <= 106`
- `arr` is **guaranteed** to be a mountain array.

```cpp
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int l=0,r=arr.size()-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(arr[m]<arr[m+1]) l=m+1;
            else r=m;
        }
        return l;
    }
}
```