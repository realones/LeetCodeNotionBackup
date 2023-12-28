# Max Pair Sum in an Array

#: 2815
Difficult: Easy
link: https://leetcode.com/problems/max-pair-sum-in-an-array/
Priority: Medium
Created time: August 12, 2023 11:02 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums`. You have to find the **maximum** sum of a pair of numbers from `nums` such that the maximum **digit** in both numbers are equal.

Return *the maximum sum or* `-1` *if no such pair exists*.

**Example 1:**

```
Input: nums = [51,71,17,24,42]
Output: 88
Explanation:
For i = 1 and j = 2, nums[i] and nums[j] have equal maximum digits with a pair sum of 71 + 17 = 88.
For i = 3 and j = 4, nums[i] and nums[j] have equal maximum digits with a pair sum of 24 + 42 = 66.
It can be shown that there are no other pairs with equal maximum digits, so the answer is 88.
```

**Example 2:**

```
Input: nums = [1,2,3,4]
Output: -1
Explanation: No pair exists in nums with equal maximum digits.

```

**Constraints:**

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 104`

Solution 1 - using pq in the contest to come up with the solution asap:

```cpp
//*to be improved by maintain just one max element instead of pq for two
class Solution {
public:
    int maxSum(vector<int>& nums) {
        int n=nums.size();
        int res=-1;
        vector<priority_queue<int,vector<int>,greater<>>> groups(10);
        for(auto x: nums){
            int maxD=maxDigit(x);
            groups[maxD].push(x);
            if(groups[maxD].size()>2) groups[maxD].pop();
        }
        for(auto& pq: groups){
            if(pq.size()!=2) continue;
            int tmp=0;
            tmp+=pq.top(); pq.pop();
            tmp+=pq.top(); pq.pop();
            res=max(res,tmp);
        }
        return res;
    }
    
    int maxDigit(int x){
        int d=0;
        while(x){
            d=max(d,x%10);
            x/=10;
        }
        return d;
    }
}
```

Solution 2 - improve with only maintain max in maxs instead of the max two integers:

```cpp
class Solution {
public:
    int maxSum(vector<int>& nums) {
        int n=nums.size();
        int res=-1;
        vector<int> maxs(10,0);//maintain the max number for each idxigit from 0 to 9
        for(auto x: nums){
            int idx=maxDigit(x);
            if(!maxs[idx]) maxs[idx]=x;
            else{
                res=max(res, maxs[idx]+x);
                maxs[idx]=max(maxs[idx],x);
            }
        }
        return res;
    }
    
    int maxDigit(int x){
        int d=0;
        while(x){
            d=max(d,x%10);
            x/=10;
        }
        return d;
    }
};
```