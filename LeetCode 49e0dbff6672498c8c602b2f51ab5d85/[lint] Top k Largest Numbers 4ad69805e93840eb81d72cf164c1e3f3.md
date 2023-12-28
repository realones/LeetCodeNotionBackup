# [lint] Top k Largest Numbers

#: 544
Difficult: Medium
Tags: PQ, TopK
link: https://www.lintcode.com/problem/top-k-largest-numbers
Priority: Pass
Created time: August 15, 2022 2:46 PM
Last edited time: October 21, 2023 4:05 PM
practicedTimes: 1
source: jiuzhang, 算初Hash&Heap

Given an integer array, find the top *k* largest numbers in it.

Example:
**Example1**

```
Input: [3, 10, 1000, -99, 4, 100] and k = 3
Output: [1000, 100, 10]

```

**Example2**

```
Input: [8, 7, 6, 5, 4, 3, 2, 1] and k = 5
Output: [8, 7, 6, 5, 4]

```

```cpp
class Solution {
public:
    /*
     * @param nums: an integer array
     * @param k: An integer
     * @return: the top k largest numbers in array
     */
    vector<int> topk(vector<int> nums, int k) {
        if(nums.size()==0 || k==0) return vector<int>();

        // write your code here
        priority_queue<int, vector<int>,greater<int>> q;
        vector<int> res;

        for(auto num:nums){
            q.push(num);
            if(q.size()>k){q.pop();}
        }
        while(!q.empty()){
            res.push_back(q.top()); q.pop();
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```