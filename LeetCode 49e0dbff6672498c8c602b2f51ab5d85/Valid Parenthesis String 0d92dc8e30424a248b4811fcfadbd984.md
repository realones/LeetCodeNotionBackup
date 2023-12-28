# Valid Parenthesis String

#: 678
Difficult: Medium
Tags: DP, Greedy, stack, string
link: https://leetcode.com/problems/valid-parenthesis-string/
Priority: High
Status: Dig More, No Idea At First, toRevisit
Created time: June 28, 2023 12:02 AM
Last edited time: December 14, 2023 7:52 PM
source: HuaHua, microsoftFreq
related: Check if There is a Path With Equal Number of 0's And 1's (Check%20if%20There%20is%20a%20Path%20With%20Equal%20Number%20of%200's%20%20a4ab620b2b9e4f19ad40a43846d02ff0.md)

Given a string `s` containing only three types of characters: `'('`, `')'` and `'*'`, return `true` *if* `s` *is **valid***.

The following rules define a **valid** string:

- Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
- Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
- Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
- `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string `""`.

**Example 1:**

```
Input: s = "()"
Output: true

```

**Example 2:**

```
Input: s = "(*)"
Output: true

```

**Example 3:**

```
Input: s = "(*))"
Output: true

```

**Constraints:**

- `1 <= s.length <= 100`
- `s[i]` is `'('`, `')'` or `'*'`.

```java
//cp from https://leetcode.com/problems/valid-parenthesis-string/solutions/543521/java-count-open-parenthesis-o-n-time-o-1-space-picture-explain/
class Solution {
    public boolean checkValidString(String s) {
        int cmin = 0, cmax = 0; // open parentheses count in range [cmin, cmax]
        for (char c : s.toCharArray()) {
            if (c == '(') {
                cmax++;
                cmin++;
            } else if (c == ')') {
                cmax--;
                cmin--;
            } else if (c == '*') {
                cmax++; // if `*` become `(` then openCount++
                cmin--; // if `*` become `)` then openCount--
                // if `*` become `` then nothing happens
                // So openCount will be in new range [cmin-1, cmax+1]
            }
            if (cmax < 0) return false; // Currently, don't have enough open parentheses to match close parentheses-> Invalid
                                        // For example: ())(
            cmin = Math.max(cmin, 0);   // It's invalid if open parentheses count < 0 that's why cmin can't be negative
        }
        return cmin == 0; // Return true if can found `openCount == 0` in range [cmin, cmax]
    }
}
```