# Greatest Common Divisor of Strings

#: 1071
Difficult: Easy
Tags: Math, string
link: https://leetcode.com/problems/greatest-common-divisor-of-strings/description/
Priority: High
Status: More Solution, No Idea At First
Created time: July 1, 2023 11:42 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

For two strings `s` and `t`, we say "`t` divides `s`" if and only if `s = t + ... + t` (i.e., `t` is concatenated with itself one or more times).

Given two strings `str1` and `str2`, return *the largest string* `x` *such that* `x` *divides both* `str1` *and* `str2`.

**Example 1:**

```
Input: str1 = "ABCABC", str2 = "ABC"
Output: "ABC"

```

**Example 2:**

```
Input: str1 = "ABABAB", str2 = "ABAB"
Output: "AB"

```

**Example 3:**

```
Input: str1 = "LEET", str2 = "CODE"
Output: ""

```

**Constraints:**

- `1 <= str1.length, str2.length <= 1000`
- `str1` and `str2` consist of English uppercase letters.

Solution:

做法太巧妙，数学方法，不会，可以看看其他做法

- str1+str2 == str2+str1 if and only if str1 and str2 have a gcd .
- E.g. str1=abcabc, str2=abc, then str1+str2 = abcabcabc = str2+str1This(str1+str2==str2+str1) is a requirement for the strings to **have a gcd**. If one of them is NOT a common part then gcd is "".It means we will return **empty stringProof**:-`str1 = mGCD, str2 = nGCD, str1 + str2 = (m + n)GCD = str2 + str1`

```cpp
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        if(str1+str2 != str2+str1) return "";
        return str1.substr(0,gcd(str1.size(), str2.size()));
    }

    //T: O(log min(a, b))
    int gcd(int a, int b){
        return b==0?a:gcd(b,a%b);
    }
};
```