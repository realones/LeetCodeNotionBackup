# Top K Frequent Elements

#: 347
Difficult: Medium
Tags: Hash, PQ, QuickSelect, TopK
link: https://leetcode.com/problems/top-k-frequent-words/
Priority: Low
Status: More Solution
Created time: May 22, 2023 11:59 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Given an integer array `nums` and an integer `k`, return *the* `k` *most frequent elements*. You may return the answer in **any order**.

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]

```

**Constraints:**

- `1 <= nums.length <= 105`
- `104 <= nums[i] <= 104`
- `k` is in the range `[1, the number of unique elements in the array]`.
- It is **guaranteed** that the answer is **unique**.

**Follow up:** Your algorithm's time complexity must be better than `O(n log n)`, where n is the array's size.

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> freq;
        for(auto a:nums){
            freq[a]++;
        }
        
        priority_queue<vector<int>,vector<vector<int>>,greater<vector<int>>> pq;
        for(auto [num, cnt]: freq){
            pq.push({cnt,num});
            if(pq.size()>k) pq.pop();
        }
        
        vector<int> res;
        
        while(!pq.empty()){
            res.push_back(pq.top()[1]); pq.pop();
        }
        
        reverse(res.begin(), res.end());
        
        return res;
    }
};
```