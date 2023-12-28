# Minimum Size Subarray Sum

#: 209
Difficult: Medium
Tags: 2pt_SameDir_minWin_chkValidAftR, Sliding Window
link: https://leetcode.com/problems/minimum-size-subarray-sum/
Priority: Low
Status: More Solution
Created time: August 20, 2022 1:35 PM
Last edited time: November 17, 2023 1:01 AM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, ticktokFreq, wisdompeak, 算高TwoPointers&PQ

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a **contiguous subarray** `[numsl, numsl+1, ..., numsr-1, numsr]` of which the sum is greater than or equal to `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.

```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1

```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0

```

**Constraints:**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

**Follow up:**

If you have figured out the

```
O(n)
```

solution, try coding another solution of which the time complexity is

```
O(n log(n))
```

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        //two pointers
        int minLen=INT_MAX;
        int sum=0;
        for(int l=0,r=0;r<nums.size();r++){
            sum+=nums[r];
            
						//if valid, update l until invalid
            while(sum>=target){
                minLen=min(minLen,r-l+1);
                sum-=nums[l];
                l++;
            }
        }
        
        
        return minLen==INT_MAX ? 0: minLen;
    }
};
```

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n=nums.size(); if(n==0) {return 0;}
        int res=INT_MAX;

        //init
        int l=0,r=0, sum=nums[0];
        if(sum>target) res=1;

        //move
        while(r<n){
            //invalid
            if(sum<target){
                //std::cout << l << "N" << r << "sum" << sum <<std::endl;
                r++;
                if(r==n) break;//corner case: out of scope
                sum+=nums[r];
            }
            //valid
            else{
                //std::cout << l << "Y" << r << "sum" << sum <<std::endl;
                res=min(res, r-l+1);
                sum-=nums[l];
                l++;
            }
        }
        return res==INT_MAX?0:res; 
    }
};
```