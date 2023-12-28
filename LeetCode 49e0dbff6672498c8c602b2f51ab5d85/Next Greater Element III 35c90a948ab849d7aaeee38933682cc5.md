# Next Greater Element III

#: 556
Difficult: Medium
Tags: 2pt, Math, monotonic, string
link: https://leetcode.com/problems/next-greater-element-iii/description/
Priority: Medium
Created time: December 21, 2023 10:49 PM
Last edited time: December 21, 2023 11:03 PM
source: googleFreq

Given a positive integer `n`, find *the smallest integer which has exactly the same digits existing in the integer* `n` *and is greater in value than* `n`. If no such positive integer exists, return `-1`.

**Note** that the returned integer should fit in **32-bit integer**, if there is a valid answer but it does not fit in **32-bit integer**, return `-1`.

**Example 1:**

```
Input: n = 12
Output: 21

```

**Example 2:**

```
Input: n = 21
Output: -1

```

**Constraints:**

- `1 <= n <= 231 - 1`

```cpp
/**
cp from https://leetcode.com/problems/next-greater-element-iii/solutions/101825/c-solution-with-explanation/ to pass
 */
class Solution {
public:
    int nextGreaterElement(int n) {
        string s = to_string(n);
        if (s.length() == 1) {
            return -1;
        }
        /* find the first decreasing digit from the right, eg: 59876, 5 is the first decreasing digit */
        int i = s.length() - 2; // 21 -> i = 0; 59876 -> i = 3
        for (; i >= 0 && s[i] >= s[i + 1]; i--) { }
        if (i == -1) {  // if a decreasing digit cannot be find, the number cannot be larger.
            return -1;
        }
        reverse(s.begin() + i + 1, s.end());
        for (int j = i + 1; j < s.length(); j++) {
            if (s[j] > s[i]) {
                swap(s[i], s[j]);
                break;
            }
        }
        long next = stol(s);
        return next == n || next > INT_MAX ? -1 : next;
    }
};

/*
mono stk:

from r->l, 

........X
        X
        X X
        X XX
        X XXT
        XXXXXX
        XXXXXXX
        XXXXXXXX
    
from r->l, find first dec elem
replace it with the smallest larger than
sort other elems in the right
*/
```