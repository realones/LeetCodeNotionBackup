# Find K-Length Substrings With No Repeated Characters

#: 1100
Difficult: Medium
Tags: Hash, Sliding Window, string
link: https://leetcode.com/problems/find-k-length-substrings-with-no-repeated-characters/
Priority: Medium
Status: Dig More
Created time: November 20, 2023 2:10 PM
Last edited time: November 20, 2023 2:11 PM
source: LC_Premium_Algo_100

Given a string `s` and an integer `k`, return *the number of substrings in* `s` *of length* `k` *with no repeated characters*.

**Example 1:**

```
Input: s = "havefunonleetcode", k = 5
Output: 6
Explanation: There are 6 substrings they are: 'havef','avefu','vefun','efuno','etcod','tcode'.

```

**Example 2:**

```
Input: s = "home", k = 5
Output: 0
Explanation: Notice k can be larger than the length of s. In this case, it is not possible to find any substring.

```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of lowercase English letters.
- `1 <= k <= 104`

Solution:

from lee215, i think it too easy to just skip and checked solution, the lee215 solution is amazing

```cpp
class Solution {
public:
    int numKLenSubstrNoRepeats(string S, int K) {
        unordered_set<int> cur;
        int res = 0, i = 0;
        for (int j = 0; j < S.size(); ++j) {
            while (cur.count(S[j]))
                cur.erase(S[i++]);
            cur.insert(S[j]);
            res += j - i + 1 >= K;
        }
        return res;
    }
};
```