# Partition Array Into Two Arrays to Minimize Sum Difference

#: 2035
Difficult: Hard
Tags: 2pt, Array, BS_todo, Bit Manipulation, BitMask, DP, OrderSet
link: https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/description/
Priority: High
Status: Dig More, HardToImplement, No Idea At First, out of scope, toSummarize, todo
Created time: December 18, 2023 7:48 PM
Last edited time: December 18, 2023 7:54 PM
source: googleFreq

You are given an integer array `nums` of `2 * n` integers. You need to partition `nums` into **two** arrays of length `n` to **minimize the absolute difference** of the **sums** of the arrays. To partition `nums`, put each element of `nums` into **one** of the two arrays.

Return *the **minimum** possible absolute difference*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/10/02/ex1.png](https://assets.leetcode.com/uploads/2021/10/02/ex1.png)

```
Input: nums = [3,9,7,3]
Output: 2
Explanation: One optimal partition is: [3,9] and [7,3].
The absolute difference between the sums of the arrays is abs((3 + 9) - (7 + 3)) = 2.

```

**Example 2:**

```
Input: nums = [-36,36]
Output: 72
Explanation: One optimal partition is: [-36] and [36].
The absolute difference between the sums of the arrays is abs((-36) - (36)) = 72.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/10/02/ex3.png](https://assets.leetcode.com/uploads/2021/10/02/ex3.png)

```
Input: nums = [2,-1,0,4,-2,-9]
Output: 0
Explanation: One optimal partition is: [2,4,-9] and [-1,0,-2].
The absolute difference between the sums of the arrays is abs((2 + 4 + -9) - (-1 + 0 + -2)) = 0.

```

**Constraints:**

- `1 <= n <= 15`
- `nums.length == 2 * n`
- `107 <= nums[i] <= 107`

Solution: **Meet In Middle**

```cpp
//from ans: https://leetcode.com/problems/partition-array-into-two-arrays-to-minimize-sum-difference/solutions/1513298/c-meet-in-middle/
class Solution {
public:
    int minimumDifference(vector<int>& nums) {
        int n = nums.size(), res = 0, sum = 0;
        sum = accumulate(nums.begin(), nums.end(),0);
        
        int N = n/2;
        vector<vector<int>> left(N+1), right(N+1);
        
		//storing all possible sum in left and right part
        for(int mask = 0; mask<(1<<N); ++mask){
            int sz = 0, l = 0, r = 0;
            for(int i=0; i<N; ++i){
                if(mask&(1<<i)){
                    sz++;
                    l += nums[i];
                    r += nums[i+N];
                }
            }
            left[sz].push_back(l);
            right[sz].push_back(r);
        }

        for(int sz=0; sz<=N; ++sz){
            sort(right[sz].begin(), right[sz].end());
        }

        res = min(abs(sum-2*left[N][0]), abs(sum-2*right[N][0]));

		//iterating over left part
        for(int sz=1; sz<N; ++sz){
            for(auto &a : left[sz]){
                int b = (sum - 2*a)/2, rsz = N-sz;
                auto &v = right[rsz];
                auto itr = lower_bound(v.begin(), v.end(),b); //binary search over right part
                
                if(itr!=v.end()) res = min(res, abs(sum-2*(a+(*itr))));
                if(itr!= v.begin()){
                    auto it = itr; --it;
                    res = min(res, abs(sum-2*(a+(*it))));
                }                
            }
        }
        return res;
        
    }
};

/*
todo: 
k sum
different data constraints, different solution
what if the questions is to take k nums instead of n/2
what if the target sum is not total/2

what is A(x,y) C(x,y)
why 25~40, calculate
*/
```