# Remove Trailing Zeros From a String

#: 2710
Difficult: Easy
Tags: string
link: https://leetcode.com/problems/remove-trailing-zeros-from-a-string/
Priority: Pass
Created time: May 27, 2023 10:30 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

Given a **positive** integer `num` represented as a string, return *the integer* `num` *without trailing zeros as a string*.

**Example 1:**

```
Input: num = "51230100"
Output: "512301"
Explanation: Integer "51230100" has 2 trailing zeros, we remove them and return integer "512301".

```

**Example 2:**

```
Input: num = "123"
Output: "123"
Explanation: Integer "123" has no trailing zeros, we return integer "123".

```

**Constraints:**

- `1 <= num.length <= 1000`
- `num` consists of only digits.
- `num` doesn't have any leading zeros.

```cpp
class Solution {
public:
    string removeTrailingZeros(string num) {
        int n=num.size();
        int i=n-1;
        while(i>=0 && num[i]=='0') i--;
        return num.substr(0,i+1);
    }
};
```