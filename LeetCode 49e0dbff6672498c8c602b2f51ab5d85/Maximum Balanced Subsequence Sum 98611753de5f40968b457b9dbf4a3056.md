# Maximum Balanced Subsequence Sum

#: 2926
Difficult: Hard
Tags: Array, BinaryIndexedTree, LinkedList, OrderMap, SegmentTree_todo, monotonic
link: https://leetcode.com/problems/maximum-balanced-subsequence-sum/
Priority: High
Status: Dig More, More Solution, No Idea At First, toSummarize
Created time: November 10, 2023 4:00 PM
Last edited time: November 11, 2023 11:15 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums`.

A **subsequence** of `nums` having length `k` and consisting of **indices** `i0 < i1 < ... < ik-1` is **balanced** if the following holds:

- `nums[ij] - nums[ij-1] >= ij - ij-1`, for every `j` in the range `[1, k - 1]`.

A **subsequence** of `nums` having length `1` is considered balanced.

Return *an integer denoting the **maximum** possible **sum of elements** in a **balanced** subsequence of* `nums`.

A **subsequence** of an array is a new **non-empty** array that is formed from the original array by deleting some (**possibly none**) of the elements without disturbing the relative positions of the remaining elements.

**Example 1:**

```
Input: nums = [3,3,5,6]
Output: 14
Explanation: In this example, the subsequence [3,5,6] consisting of indices 0, 2, and 3 can be selected.
nums[2] - nums[0] >= 2 - 0.
nums[3] - nums[2] >= 3 - 2.
Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.
The subsequence consisting of indices 1, 2, and 3 is also valid.
It can be shown that it is not possible to get a balanced subsequence with a sum greater than 14.
```

**Example 2:**

```
Input: nums = [5,-1,-3,8]
Output: 13
Explanation: In this example, the subsequence [5,8] consisting of indices 0 and 3 can be selected.
nums[3] - nums[0] >= 3 - 0.
Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.
It can be shown that it is not possible to get a balanced subsequence with a sum greater than 13.

```

**Example 3:**

```
Input: nums = [-2,-1]
Output: -1
Explanation: In this example, the subsequence [-1] can be selected.
It is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.

```

**Constraints:**

- `1 <= nums.length <= 105`
- `109 <= nums[i] <= 109`

**Solution**:

```cpp
using ll=long long;

class Solution {
public:
    long long maxBalancedSubsequenceSum(vector<int>& nums) {
        int n=nums.size();//1~1e5
        //preprocess
        vector<int> arr(nums);
        for(int i=0; i<n; i++){arr[i]-=i;}
        //then find a val inc subseq in arr
        //that corresponding nums elem sum is maximum
        map<int,ll> mp{{INT_MIN,0}};//init 
        for(int i=0; i<n; i++){
            ll sum = max(prev(mp.upper_bound(arr[i]))->second,0LL) + nums[i];
            mp[arr[i]]=sum;
            auto it=mp.upper_bound(arr[i]);//nxt one
            while(it!=mp.end() && it->second <= sum) mp.erase(it++);
        }
        return mp.rbegin()->second;
    }
};

/*
i     0 1 2 3 4 5
arr   4 3 6 4 3 5
        ^   ^   ^   <= inc   
nums  X X X X X X
        ^   ^   ^   <= sum is max

dp[i]: max sum of nums elem, with val inc subseq from arr, end with i

dp[i] max= dp[j]+nums[i], j:0~i-1, arr[j]<arr[i]

init: dp[i]=nums[i]

return: max of dp[i], i:0~n-1

T: O(n^2) -> TLE

================================
monotonic:
    for visited:
        monotonic quality:
            arrVal - the smaller the better
            sum - the larger the better
        so, maintain linked list/ map - pair(arrVal,sum)
            arrVal - small to large
            sum - small to large
        when new pair<arrVal, sum> comes, it will remove less qualified ones
            less qualified - ones with value larger but sum lessOrEqual
    for cur number:
        find the one to use, +=x.numVal:
            dp(x) = dp.upper_bound(x).prev.sum + x.numVal
        insert sum(x) back to the list/map, remove less qualifed ones on the right of its position

container:
    feature:
        sorted 
    operation:
        insert - 1.find(logn) 2.insert(list:log1/ map logn)
        find - BS - logn

================================
nums [X X X X X] X
        ^   ^    ^   <= sum is max
arr  [4 3 6 4 3] 5
        ^   ^    ^   <= inc   
dp   [. . 7 3 5] ?
idx              i

monotonic: always use arr val smaller, and dp[val] larger

dp[x]: max sum (on nums), with val inc subseq (on arr) , end with val x

dp[x] max= dp[y]+nums[i], y: last arr val<=x ,dp is sorted in map

init(same): dp[i]=nums[i]

return(same): max of dp[i], i:0~n-1

T: O(nlogn)
*/
```