# Summary Ranges

#: 228
Difficult: Easy
Tags: 2pt, Array, Interval
link: https://leetcode.com/problems/summary-ranges/
Priority: High
Created time: June 2, 2023 11:56 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

You are given a **sorted unique** integer array `nums`.

A **range** `[a,b]` is the set of all integers from `a` to `b` (inclusive).

Return *the **smallest sorted** list of ranges that **cover all the numbers in the array exactly***. That is, each element of `nums` is covered by exactly one of the ranges, and there is no integer `x` such that `x` is in one of the ranges but not in `nums`.

Each range `[a,b]` in the list should be output as:

- `"a->b"` if `a != b`
- `"a"` if `a == b`

**Example 1:**

```
Input: nums = [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: The ranges are:
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"

```

**Example 2:**

```
Input: nums = [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: The ranges are:
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"

```

**Constraints:**

- `0 <= nums.length <= 20`
- `231 <= nums[i] <= 231 - 1`
- All the values of `nums` are **unique**.
- `nums` is sorted in ascending order.

**Compare the diff and same of following solutions**

Solution 1: 

```cpp
lass Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        int n=nums.size(); if(n==0) {return {};}
        vector<string> res;
        for(int l=0,r=0;l<n && r<n;){
            while(r+1<n && nums[r+1]==nums[r]+1) r++;
            res.push_back(
                l==r?to_string(
                    nums[l]): 
                    to_string(nums[l])+"->"+to_string(nums[r])
            );
            l=r+1,r=l;
        }
        return res;
    }
};
```

Solution 2:

```cpp
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> res;
        int n=nums.size(); if(n==0) return res;
        for(int l=0,r=0;r<n;r++){
            //if block ends
            if(r==n-1 || nums[r]+1!=nums[r+1]){
                res.push_back(to_string(nums[l]));
                if(l!=r){
                    res.back()+="->"+to_string(nums[r]);
                }
                l=r+1;
            }
        }
        return res;
    }
};
```