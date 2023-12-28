# Sum of Subarray Minimums

#: 907
Difficult: Medium
Tags: Array, DP, monotonic, stack
link: https://leetcode.com/problems/sum-of-subarray-minimums/description/
Priority: High
Status: HardToImplement, No Idea At First, toRevisit, toSummarize
Created time: September 11, 2023 8:42 PM
Last edited time: October 17, 2023 6:24 PM
source: HuaHua

Given an array of integers arr, find the sum of `min(b)`, where `b` ranges over every (contiguous) subarray of `arr`. Since the answer may be large, return the answer **modulo** `109 + 7`.

**Example 1:**

```
Input: arr = [3,1,2,4]
Output: 17
Explanation:
Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4].
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.
Sum is 17.

```

**Example 2:**

```
Input: arr = [11,81,94,43,3]
Output: 444

```

**Constraints:**

- `1 <= arr.length <= 3 * 104`
- `1 <= arr[i] <= 3 * 104`

Solution:

```cpp
//https://leetcode.com/problems/sum-of-subarray-minimums/solutions/178876/stack-solution-with-very-detailed-explanation-step-by-step/
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {
        int n=arr.size();//1~3e4
        vector<int> left(n);//dist to PLE
        vector<int> right(n);//dist to NLE
        //init
        for(int i=0; i<n; i++) left[i]=i+1;
        for(int i=0; i<n; i++) right[i]=n-i;

        stack<int> stk;//inc
        for(int i=0; i<n; i++){
            while(!stk.empty() && arr[stk.top()]>arr[i]){
                right[stk.top()] = i-stk.top();
                stk.pop();
            }
            if(!stk.empty()) left[i]=i-stk.top();
            stk.push(i);
        }

        ll res=0;
        for(int i=0; i<n; i++){
            res += ((ll)left[i]*arr[i]*right[i])%M;
            res %= M;
        }
        return res;
    }
};
```