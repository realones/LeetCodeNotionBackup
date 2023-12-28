# [lint] Search in a Big Sorted Array

#: 447
Difficult: Medium
Tags: BS_Idx
link: https://www.lintcode.com/problem/search-in-a-big-sorted-array
Priority: Low
Created time: June 26, 2022 10:59 AM
Last edited time: October 12, 2023 2:57 PM
practicedTimes: 1
source: jiuzhang, 算初BS

Tags:   lintcode   google   Sorted Array   Binary Search   LintCode Copyright

- lintcode
- Medium (36.00%)
- Total Accepted: 27000
- Total Submissions: 73220
- Testcase Example: '[1,3,6,9,21]\n3\n'

Given a big sorted array with non-negative integers sorted by non-decreasing order. The array is so big so that you can not get the length of the whole array directly, and you can only access the kth number by `ArrayReader.get(k)` (or ArrayReader->get(k) for C++).

Find the first index of a target number. Your algorithm should be in O(log k), where k is the first index of the target number.

Return -1, if the number doesn't exist in the array.

Example:
**Example 1:**

```
Input: [1, 3, 6, 9, 21, ...], target = 3
Output: 1

```

**Example 2:**

```
Input: [1, 3, 6, 9, 21, ...], target = 4
Output: -1

```

Challenge
O(logn) time, n is the first index of the given target number.

Related Questions:
first-position-of-target

Tags:
Sorted Array,Binary Search,LintCode Copyright

```cpp
class Solution {
public:
    /*
     * @param reader: An instance of ArrayReader.
     * @param target: An integer
     * @return: An integer which is the first index of target.
     */
    int searchBigSortedArray(ArrayReader * reader, int target) {
        // double
        int sz=1;
        for(;reader->get(sz-1)<target;sz*=2);
        //BS
        int l=0,r=sz-1;
        while(l<=r){
            int m=l+(r-l)/2;
            if(reader->get(m)<target) l=m+1;
            else r=m-1;
        }
        if(l==sz) return -1;
        if(reader->get(l)==target) return l;
        return -1;
    }
};
```