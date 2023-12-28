# Find Unique Binary String

#: 1980
Difficult: Medium
Tags: Array, BackTracking, Bit Manipulation, string
link: https://leetcode.com/problems/find-unique-binary-string
Priority: Medium
Status: HardToImplement, More Solution
Created time: November 16, 2023 2:18 AM
Last edited time: November 16, 2023 2:20 AM
source: leetcode_daily

Given an array of strings `nums` containing `n` **unique** binary strings each of length `n`, return *a binary string of length* `n` *that **does not appear** in* `nums`*. If there are multiple answers, you may return **any** of them*.

**Example 1:**

```
Input: nums = ["01","10"]
Output: "11"
Explanation: "11" does not appear in nums. "00" would also be correct.

```

**Example 2:**

```
Input: nums = ["00","01"]
Output: "11"
Explanation: "11" does not appear in nums. "10" would also be correct.

```

**Example 3:**

```
Input: nums = ["111","011","001"]
Output: "101"
Explanation: "101" does not appear in nums. "000", "010", "100", and "110" would also be correct.

```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 16`
- `nums[i].length == n`
- `nums[i]` is either `'0'` or `'1'`.
- All the strings of `nums` are **unique**.

todo: back tracking solution

Solution:

非常好想，但是出乎意料submit错了很多次，位运算pri非常低，注意括号

```cpp
class Solution {
public:
    string findDifferentBinaryString(vector<string>& nums) {
        int n=nums.size();
        unordered_set<string> st(nums.begin(), nums.end());
        for(int i=0;i<pow(2,n);i++){
            string s(n,'0');
            for(int d=0;d<n;d++){
                s[d]+= (i&(1<<d))!=0;
            }
            if(!st.count(s)) { return s; }
        }
        return "not_found";
    }
};
```