# Maximum Subsequence Score

#: 2542
Difficult: Medium
Tags: Array, Greedy, PQ, Sort
link: https://leetcode.com/problems/maximum-subsequence-score/
Priority: High
Created time: May 24, 2023 7:08 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75, leetcode_daily

You are given two **0-indexed** integer arrays `nums1` and `nums2` of equal length `n` and a positive integer `k`. You must choose a **subsequence** of indices from `nums1` of length `k`.

For chosen indices `i0`, `i1`, ..., `ik - 1`, your **score** is defined as:

- The sum of the selected elements from `nums1` multiplied with the **minimum** of the selected elements from `nums2`.
- It can defined simply as: `(nums1[i0] + nums1[i1] +...+ nums1[ik - 1]) * min(nums2[i0] , nums2[i1], ... ,nums2[ik - 1])`.

Return *the **maximum** possible score.*

A **subsequence** of indices of an array is a set that can be derived from the set `{0, 1, ..., n-1}` by deleting some or no elements.

**Example 1:**

```
Input: nums1 = [1,3,3,2], nums2 = [2,1,3,4], k = 3
Output: 12
Explanation:
The four possible subsequence scores are:
- We choose the indices 0, 1, and 2 with score = (1+3+3) * min(2,1,3) = 7.
- We choose the indices 0, 1, and 3 with score = (1+3+2) * min(2,1,4) = 6.
- We choose the indices 0, 2, and 3 with score = (1+3+2) * min(2,3,4) = 12.
- We choose the indices 1, 2, and 3 with score = (3+3+2) * min(1,3,4) = 8.
Therefore, we return the max score, which is 12.

```

**Example 2:**

```
Input: nums1 = [4,2,3,1,1], nums2 = [7,5,10,9,6], k = 1
Output: 30
Explanation:
Choosing index 2 is optimal: nums1[2] * nums2[2] = 3 * 10 = 30 is the maximum possible score.

```

**Constraints:**

- `n == nums1.length == nums2.length`
- `1 <= n <= 105`
- `0 <= nums1[i], nums2[j] <= 105`
- `1 <= k <= n`

solution: similar to dp, calculate range [0,right], nums2[right] must included, right++

```cpp
class Solution {
public:
    long long maxScore(vector<int>& nums1, vector<int>& nums2, int k) {
        int n=nums1.size();
        //sort based on nums2
        vector<vector<int>> m(n);
        for(int i=0; i<n; i++){
            m[i]={nums2[i],nums1[i]};
        }
        sort(m.begin(),m.end(),greater());
        
        //for each idx
        long long res=INT_MIN;
        priority_queue<int,vector<int>,greater<>> pq;
        long long sum=0;
        for(int i=0;i<n;i++){
            int num2=m[i][0];
            int num1=m[i][1];
            
            if(i-1>=0) {
                pq.push(m[i-1][1]);
                sum+=m[i-1][1];
            }
            if(pq.size()>k-1) {
                sum-=pq.top();
                pq.pop();
            }
            
            if(i>=k-1){
                res=max(res,(sum+num1)*num2);
            }
        }
        return res;
    }
};

/*
k=3

0 1 2 3 min
*
8 8 0 0 sum

max of ((a1 + a2 + a3 +...ak) * min(b1, b2, b3, ...bk))

bind nums1 and nums2
sort based on nums2 - large to small

for each idx (num1 ,num2)
    res max= (sum of (k-1) pre num1 + num1) * num2
    
solve (sum of (k-1) pre num1 + num1):
    maintain k-1 element max sum up to i-1
    <-
    maintain k element max sum up to i
    <-
    maintain k largest elements up to i
    pq of largest k element, 
*/
```