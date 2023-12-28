# Range Sum of Sorted Subarray Sums

#: 1508
Difficult: Medium
Tags: 2pt, BS_todo, PQ, Prefix
link: https://leetcode.com/problems/range-sum-of-sorted-subarray-sums/
Priority: Low
Status: More Solution, checkBetterSolution
Created time: June 25, 2023 4:26 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given the array `nums` consisting of `n` positive integers. You computed the sum of all non-empty continuous subarrays from the array and then sorted them in non-decreasing order, creating a new array of `n * (n + 1) / 2` numbers.

*Return the sum of the numbers from index* `left` *to index* `right` (**indexed from 1**)*, inclusive, in the new array.* Since the answer can be a huge number return it modulo `109 + 7`.

**Example 1:**

```
Input: nums = [1,2,3,4], n = 4, left = 1, right = 5
Output: 13
Explanation: All subarray sums are 1, 3, 6, 10, 2, 5, 9, 3, 7, 4. After sorting them in non-decreasing order we have the new array [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]. The sum of the numbers from index le = 1 to ri = 5 is 1 + 2 + 3 + 3 + 4 = 13.

```

**Example 2:**

```
Input: nums = [1,2,3,4], n = 4, left = 3, right = 4
Output: 6
Explanation: The given array is the same as example 1. We have the new array [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]. The sum of the numbers from index le = 3 to ri = 4 is 3 + 3 = 6.

```

**Example 3:**

```
Input: nums = [1,2,3,4], n = 4, left = 1, right = 10
Output: 50

```

**Constraints:**

- `n == nums.length`
- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 100`
- `1 <= left <= right <= n * (n + 1) / 2`

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int rangeSum(vector<int>& nums, int n, int left, int right) {
        vector<ll> prefixSum(nums.begin(), nums.end());
        for(int i=1;i<n;i++){
            prefixSum[i]+=prefixSum[i-1];
        }

        return (firstKthSum(prefixSum,n,right) - firstKthSum(prefixSum,n,left-1) + M) % M;
    }

    ll firstKthSum(vector<ll>& prefixSum, int n, int k){
        priority_queue<ll> pq;
        for(int i=0;i<n;i++){
            for(int j=i;j<n;j++){
                ll sum=getPrefixSum(prefixSum,i,j);
                pq.push(sum);
                if(pq.size()>k) pq.pop();
            }
        }
        ll sum=0;
        while(!pq.empty()){
            ll cur=pq.top(); pq.pop();
            sum=(sum+cur)%M;
        }
        return (sum+M)%M;
    }

    ll getPrefixSum(vector<ll>& prefixSum, ll l, ll r){
        return prefixSum[r] - (l==0?0:prefixSum[l-1]);
    }
};
```