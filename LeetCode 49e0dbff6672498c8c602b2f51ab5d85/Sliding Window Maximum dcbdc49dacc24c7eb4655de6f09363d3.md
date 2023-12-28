# Sliding Window Maximum

#: 239
Difficult: Hard
Tags: Sliding Window, deque, monotonic
link: https://leetcode.com/problems/sliding-window-maximum/
Priority: Medium
Created time: September 9, 2022 12:03 PM
Last edited time: October 29, 2023 5:01 PM
practicedTimes: 1
source: jiuzhang, 算高BS&SwipeLine

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the max sliding window*.

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation:
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  73
 1 [3  -1  -3] 5  3  6  73
 1  3 [-1  -3  5] 3  6  7 5
 1  3  -1 [-3  5  3] 6  75
 1  3  -1  -3 [5  3  6] 76
 1  3  -1  -3  5 [3  6  7]7
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]

```

**Constraints:**

- `1 <= nums.length <= 105`
- `104 <= nums[i] <= 104`
- `1 <= k <= nums.length`

Solution1: sliding window, 2pt same speed, trim left then add cur

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n=nums.size(); if(n==0) {return {};}
        vector<int> res;
        deque<int> dq;
        for(int i=0; i<n; i++){
            //pop left when i>=k
            if(i>=k){
                if(dq.back() == nums[i-k]) dq.pop_back();
            }
            //for each in q
            while(!dq.empty() && dq.front()<nums[i]) dq.pop_front();
            dq.push_front(nums[i]);
            //to res when i>=k-1
            if(i>=k-1){
                res.push_back(dq.back());
            }
        }
        return res;
    }
};
```

Solution1: sliding window, 2pt same speed, add cur then trim left

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n=nums.size(); if(n==0) {return {};}
        vector<int> res;
        deque<int> dq;
        for(int i=0; i<n; i++){
            //for each in q
            while(!dq.empty() && dq.front()<nums[i]) dq.pop_front();
            dq.push_front(nums[i]);
            //pop left when i>=k
            if(i>=k){
                if(dq.back() == nums[i-k]) dq.pop_back();
            }
            //to res when i>=k-1
            if(i>=k-1){
                res.push_back(dq.back());
            }
        }
        return res;
    }
};
```