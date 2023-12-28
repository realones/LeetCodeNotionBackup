# Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit

#: 1438
Difficult: Medium
Tags: 2pt_SameDir_maxWin_chkValidAftR, Array, Heap, OrderSet, deque, monotonic
link: https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/description/
Priority: High
Status: Dig More, More Solution
Created time: June 23, 2023 4:35 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an array of integers `nums` and an integer `limit`, return the size of the longest **non-empty** subarray such that the absolute difference between any two elements of this subarray is less than or equal to `limit`*.*

**Example 1:**

```
Input: nums = [8,2,4,7], limit = 4
Output: 2
Explanation: All subarrays are:
[8] with maximum absolute diff |8-8| = 0 <= 4.
[8,2] with maximum absolute diff |8-2| = 6 > 4.
[8,2,4] with maximum absolute diff |8-2| = 6 > 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
[2] with maximum absolute diff |2-2| = 0 <= 4.
[2,4] with maximum absolute diff |2-4| = 2 <= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 > 4.
[4] with maximum absolute diff |4-4| = 0 <= 4.
[4,7] with maximum absolute diff |4-7| = 3 <= 4.
[7] with maximum absolute diff |7-7| = 0 <= 4.
Therefore, the size of the longest subarray is 2.

```

**Example 2:**

```
Input: nums = [10,1,2,4,7,2], limit = 5
Output: 4
Explanation: The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 <= 5.

```

**Example 3:**

```
Input: nums = [4,2,2,2,4,4,2,2], limit = 0
Output: 3

```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`
- `0 <= limit <= 109`

Solution 1: O(n)

Max Window(find range) + Mono Deque(maintain min/max elements)

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        int n=nums.size();
        int res=1;
        deque<int> decQ, incQ;
        for(int l=0,r=0;r<n;r++){
            //add r
            while(!decQ.empty() && decQ.front()<nums[r]) decQ.pop_front();
            decQ.push_front(nums[r]);
            while(!incQ.empty() && incQ.front()>nums[r]) incQ.pop_front();
            incQ.push_front(nums[r]);
            //while new r cause invliad, pop l
            while(decQ.back() - incQ.back()>limit){
                if(decQ.back()==nums[l]) decQ.pop_back();
                if(incQ.back()==nums[l]) incQ.pop_back();
                l++;
            }
            res=max(res,r-l+1);
        }
        return res;
    }
};

/*
[8,2,4,7]

max window 
maintain window max and min

mon dec q: manitain max ele
mon inc q: manitain min ele

max window
*/
```

Solution 2: O(nlogn)

Max Window(find range) + Multi Tree Set(maintain min/max elements)

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        int n=nums.size();
        int res=1;
        multiset<int> st;
        for(int l=0,r=0;r<n;r++){
            st.insert(nums[r]);
            //while r cause invalid, pop l
            while(*st.rbegin()-*st.begin()>limit){
                st.erase(st.find(nums[l]));
                l++;
            }
            res=max(res,r-l+1);
        }
        return res;
    }
};
```