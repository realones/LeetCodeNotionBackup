# Find Palindrome With Fixed Length

#: 2217
Difficult: Medium
Tags: Array, Math
link: https://leetcode.com/problems/find-palindrome-with-fixed-length/description/
Priority: Low
Created time: December 6, 2023 3:44 PM
Last edited time: December 6, 2023 3:45 PM
source: ticktokFreq

Given an integer array `queries` and a **positive** integer `intLength`, return *an array* `answer` *where* `answer[i]` *is either the* `queries[i]th` *smallest **positive palindrome** of length* `intLength` *or* `-1` *if no such palindrome exists*.

A **palindrome** is a number that reads the same backwards and forwards. Palindromes cannot have leading zeros.

**Example 1:**

```
Input: queries = [1,2,3,4,5,90], intLength = 3
Output: [101,111,121,131,141,999]
Explanation:
The first few palindromes of length 3 are:
101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 202, ...
The 90th palindrome of length 3 is 999.

```

**Example 2:**

```
Input: queries = [2,4,6], intLength = 4
Output: [1111,1331,1551]
Explanation:
The first six palindromes of length 4 are:
1001, 1111, 1221, 1331, 1441, and 1551.

```

**Constraints:**

- `1 <= queries.length <= 5 * 104`
- `1 <= queries[i] <= 109`
- `1 <= intLength <= 15`

```cpp
using ll=long long;

class Solution {
public:
    vector<long long> kthPalindrome(vector<int>& queries, int intLength) {
        bool cutOne = (intLength%2==1);
        int halfSz=(intLength+1)/2;
        string initS="1"+string(halfSz-1,'0');//build 100000
        ll init=stoi(initS);
        auto trans = [&](ll half)->ll{
          string left=to_string(half);
          if(left.size()>initS.size()) return -1;
          string right=left;
          if(cutOne) right.pop_back();
          reverse(right.begin(), right.end());
          return stol(left+right);
        };
        vector<ll> res;
        for(auto q: queries){
          res.push_back(trans(init+q-1));
        }
        return res;
    }
};

/*
queries:
  len: 1~5e4
  val: 1~1e9
len: 1~15
================================
100000001
mid rise first
then near by two elem
==>
so cut in half, we take left half, 1000000
try to raise it from smallest digit(rightmost)
kth -> 100000 + k-1
*/
```