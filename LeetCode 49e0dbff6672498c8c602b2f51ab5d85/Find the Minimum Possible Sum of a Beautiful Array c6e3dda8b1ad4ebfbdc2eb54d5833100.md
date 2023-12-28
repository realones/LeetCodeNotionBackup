# Find the Minimum Possible Sum of a Beautiful Array

#: 2834
Difficult: Medium
Tags: Hash
link: https://leetcode.com/problems/find-the-minimum-possible-sum-of-a-beautiful-array/description/
Priority: Low
Status: dup
Created time: August 26, 2023 9:42 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests
related: Determine the Minimum Sum of a k-avoiding Array (Determine%20the%20Minimum%20Sum%20of%20a%20k-avoiding%20Array%20555d1637c8d847c0b692215cae23ba97.md)

You are given positive integers `n` and `target`.

An array `nums` is **beautiful** if it meets the following conditions:

- `nums.length == n`.
- `nums` consists of pairwise **distinct** **positive** integers.
- There doesn't exist two **distinct** indices, `i` and `j`, in the range `[0, n - 1]`, such that `nums[i] + nums[j] == target`.

Return *the **minimum** possible sum that a beautiful array could have*.

**Example 1:**

```
Input: n = 2, target = 3
Output: 4
Explanation: We can see that nums = [1,3] is beautiful.
- The array nums has length n = 2.
- The array nums consists of pairwise distinct positive integers.
- There doesn't exist two distinct indices, i and j, with nums[i] + nums[j] == 3.
It can be proven that 4 is the minimum possible sum that a beautiful array could have.

```

**Example 2:**

```
Input: n = 3, target = 3
Output: 8
Explanation: We can see that nums = [1,3,4] is beautiful.
- The array nums has length n = 3.
- The array nums consists of pairwise distinct positive integers.
- There doesn't exist two distinct indices, i and j, with nums[i] + nums[j] == 3.
It can be proven that 8 is the minimum possible sum that a beautiful array could have.

```

**Example 3:**

```
Input: n = 1, target = 1
Output: 1
Explanation: We can see, that nums = [1] is beautiful.

```

**Constraints:**

- `1 <= n <= 105`
- `1 <= target <= 105`

```cpp
using ll=long long;

class Solution {
public:
    long long minimumPossibleSum(int n, int target) {
        ll res=0;
        unordered_set<ll> st;
        ll x=1;
        for(int i=0; i<n; i++){
            res+=x;
            st.insert(target-x);
            x++;
            while(st.count(x)) x++;
        }
        return res;
    }
};
/*
if nums.len==n
num:  pairwise distinct positive int
no i and j, that i!=j && nums[i] + nums[j] == target

return the min sum of all possible arrays
================================
*/
```