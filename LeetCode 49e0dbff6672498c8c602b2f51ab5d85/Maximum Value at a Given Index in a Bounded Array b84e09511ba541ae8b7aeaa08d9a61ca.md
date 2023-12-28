# Maximum Value at a Given Index in a Bounded Array

#: 1802
Difficult: Medium
Tags: BS_Val, Math
link: https://leetcode.com/problems/maximum-value-at-a-given-index-in-a-bounded-array/
Priority: Medium
Created time: June 10, 2023 4:51 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given three positive integers: `n`, `index`, and `maxSum`. You want to construct an array `nums` (**0-indexed**) ****that satisfies the following conditions:

- `nums.length == n`
- `nums[i]` is a **positive** integer where `0 <= i < n`.
- `abs(nums[i] - nums[i+1]) <= 1` where `0 <= i < n-1`.
- The sum of all the elements of `nums` does not exceed `maxSum`.
- `nums[index]` is **maximized**.

Return `nums[index]` *of the constructed array*.

Note that `abs(x)` equals `x` if `x >= 0`, and `-x` otherwise.

**Example 1:**

```
Input: n = 4, index = 2,  maxSum = 6
Output: 2
Explanation: nums = [1,2,2,1] is one array that satisfies all the conditions.
There are no arrays that satisfy all the conditions and have nums[2] == 3, so 2 is the maximum nums[2].

```

**Example 2:**

```
Input: n = 6, index = 1,  maxSum = 10
Output: 3

```

**Constraints:**

- `1 <= n <= maxSum <= 109`
- `0 <= index < n`

```cpp
class Solution {
public:
    int maxValue(int n, int index, int maxSum) {
        int l=1, r=maxSum;
        while(l<r){
            int m=l+((long long)r-l+1)/2;
            if(!valid(n,index,maxSum,m)){r=m-1;}
            else{l=m;}
        }
        return r;
    }
    
    //find the max val that the min of sum last<= maxSum
    bool valid(int n, int idx, int maxSum, int x){
        double lsz=idx, rsz=n-1-idx;
        //l
        double l=0;
        if(idx-x+1>=0){
            l+=0.5*(x-1)*x;
            l+=(lsz-x+1)*1;
        }
        else{
            l+=0.5*lsz*(x-lsz + x-1);
        }
        //r
        double r=0;
        if(idx+x-1<n){
            r+=0.5*(x-1)*x;
            r+=(rsz-x+1)*1;
        }
        else{
            r+=0.5*rsz*(x-rsz + x-1);
        }
        
        double minSum=l+x+r;
        return minSum<=maxSum;
    }
};
```