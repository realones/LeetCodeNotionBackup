# Bitwise ORs of Subarrays

#: 898
Difficult: Medium
Tags: Bit Manipulation, DP, Hash
link: https://leetcode.com/problems/bitwise-ors-of-subarrays/
Priority: High
Status: No Idea At First
Created time: June 15, 2023 6:42 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily

Given an integer array `arr`, return *the number of distinct bitwise ORs of all the non-empty subarrays of* `arr`.

The bitwise OR of a subarray is the bitwise OR of each integer in the subarray. The bitwise OR of a subarray of one integer is that integer.

A **subarray** is a contiguous non-empty sequence of elements within an array.

**Example 1:**

```
Input: arr = [0]
Output: 1
Explanation: There is only one possible result: 0.

```

**Example 2:**

```
Input: arr = [1,1,2]
Output: 3
Explanation: The possible subarrays are [1], [1], [2], [1, 1], [1, 2], [1, 1, 2].
These yield the results 1, 1, 2, 1, 3, 3.
There are 3 unique values, so the answer is 3.

```

**Example 3:**

```
Input: arr = [1,2,4]
Output: 6
Explanation: The possible results are 1, 2, 3, 4, 6, and 7.

```

**Constraints:**

- `1 <= arr.length <= 5 * 104`
- `0 <= arr[i] <= 109`

**Solution**:

[https://leetcode.com/problems/bitwise-ors-of-subarrays/discuss/165881/C%2B%2BJavaPython-O(30N)](https://leetcode.com/problems/bitwise-ors-of-subarrays/discuss/165881/C%2B%2BJavaPython-O(30N))

**Intuition**:
Assume B[i][j] = A[i] | A[i+1] | ... | A[j]
Hash set cur stores all wise B[0][i], B[1][i], B[2][i], B[i][i].

When we handle the A[i+1], we want to update cur
So we need operate bitwise OR on all elements in cur.
Also we need to add A[i+1] to cur.

In each turn, we add all elements in cur to res.

**Complexity**:
Time O(30N)

Normally this part is easy.
But for this problem, time complexity matters a lot.

The solution is straight forward,
while you may worry about the time complexity up to O(N^2)
However, it's not the fact.
This solution has only O(30N)

The reason is that, B[0][i] >= B[1][i] >= ... >= B[i][i].
B[0][i] covers all bits of B[1][i]
B[1][i] covers all bits of B[2][i]
....

There are at most 30 bits for a positive number 0 <= A[i] <= 10^9.
So there are at most 30 different values for B[0][i], B[1][i], B[2][i], ..., B[i][i].
Finally cur.size() <= 30 and res.size() <= 30 * A.length()

In a worst case, A = {1,2,4,8,16,..., 2 ^ 29}
And all B[i][j] are different and res.size() == 30 * A.length()

```cpp
class Solution {
public:
    int subarrayBitwiseORs(vector<int>& nums) {
        unordered_set<int> res, cur, cur2;
        for(auto num: nums){//1..n-1
            cur2={num};
            for(auto a: cur) cur2.insert(num|a);
            cur=cur2;
            for(auto a: cur) res.insert(a);
        }
        return res.size();
    }
};
```