# [lint] Remove Duplicate Numbers in Array

#: 521
Difficult: Easy
Tags: 2pt_SameDir_diffSpeed, Array
link: https://www.lintcode.com/problem/remove-duplicate-numbers-in-array
Priority: Low
Created time: August 3, 2022 1:37 PM
Last edited time: October 19, 2023 9:50 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

Given an array of integers, remove the duplicate numbers in it.

You should:

1. Do it in place in the array.
2. Move the unique numbers to the front of the array.
3. Return the total number of the unique numbers.

Example:
Example 1:

```
Input:
nums = [1,3,1,4,4,2]
Output:
[1,3,4,2,?,?]
4

```

Explanation:

1. Move duplicate integers to the tail of *nums* => *nums* = `[1,3,4,2,?,?]`.
2. Return the number of unique integers in *nums* => `4`.
Actually we don't care about what you place in `?`, we only care about the part which has no duplicate integers.

Example 2:

```
Input:
nums = [1,2,3]
Output:
[1,2,3]
3

```

Challenge

1. Do it in O(n) time complexity.
2. Do it in O(nlogn) time without extra space.

Related Questions:
distinguish-username,name-deduplication

set1

```cpp
class Solution {
public:
    int deduplication(vector<int> &nums) {
        // write your code here
        int n=nums.size(); if(n==0) {return 0;}
        unordered_set<int> st;
        int k=0;
        for(int i=0; i<n; i++){
            if(st.count(nums[i])){
                continue;
            }
            nums[k++]=nums[i];
            st.insert(nums[i]);
        }
				return k;
    }

```

set2

```cpp
class Solution {
public
    int deduplication(vector<int> &nums) {
        // write your code here
        int n=nums.size(); if(n==0) {return 0;}
        unordered_set<int> st(nums.begin(),nums.end());
        int res=st.size();
        int i=0;
        for(auto num: st){
            nums[i++]=num;
        }
        return res;
    }
    }
};
```

sort

```cpp
class Solution {
public:
    int deduplication(vector<int>& nums) {
        // Write your code here
        int n = nums.size(), m = 0;
        if (n == 0)
            return 0;

        sort(nums.begin(), nums.end());
        for (int i = 0; i < n; ++i) {
            if(i==0 || nums[i]!=nums[i-1]){
                nums[m++]=nums[i];
            }
        }
        return m;
    }
};
```