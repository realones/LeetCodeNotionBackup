# Longest Consecutive Sequence

#: 128
Difficult: Medium
Tags: Hash, Union Find
link: https://leetcode.com/problems/longest-consecutive-sequence/
Priority: Medium
Status: More Solution
Created time: August 11, 2022 7:19 PM
Last edited time: October 21, 2023 1:21 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算初DP, 算初Hash&Heap

Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is[1, 2, 3, 4]. Therefore its length is 4.

```

**Example 2:**

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9

```

**Constraints:**

- `0 <= nums.length <= 105`
- `109 <= nums[i] <= 109`

时间复杂度不变 ary可以变set set可以更改 注意set o1的get速度

change input
union find？- since not 1by1, so not need unionFind

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int res=0;
        unordered_set<int> set(nums.begin(),nums.end());
        while(!set.empty()){
            int val=*set.begin(); set.erase(set.begin());
            int pre=val-1, nxt=val+1;
            while(set.count(pre)) {set.erase(pre); pre--;}
            while(set.count(nxt)) {set.erase(nxt); nxt++;}
            res=max(res, nxt-pre-1); 
        }
        return res;
    }
};
```