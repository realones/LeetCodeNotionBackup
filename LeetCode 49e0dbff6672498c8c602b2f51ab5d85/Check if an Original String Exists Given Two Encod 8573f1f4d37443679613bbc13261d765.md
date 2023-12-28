# Check if an Original String Exists Given Two Encoded Strings

#: 2060
Difficult: Hard
Tags: DP, recursion, string
link: https://leetcode.com/problems/check-if-an-original-string-exists-given-two-encoded-strings/description/
Priority: High
Status: HardToImplement, No Idea At First, toRevisit
Created time: December 6, 2023 6:53 PM
Last edited time: December 6, 2023 6:56 PM
source: facebookFreq

An original string, consisting of lowercase English letters, can be encoded by the following steps:

- Arbitrarily **split** it into a **sequence** of some number of **non-empty** substrings.
- Arbitrarily choose some elements (possibly none) of the sequence, and **replace** each with **its length** (as a numeric string).
- **Concatenate** the sequence as the encoded string.

For example, **one way** to encode an original string `"abcdefghijklmnop"` might be:

- Split it as a sequence: `["ab", "cdefghijklmn", "o", "p"]`.
- Choose the second and third elements to be replaced by their lengths, respectively. The sequence becomes `["ab", "12", "1", "p"]`.
- Concatenate the elements of the sequence to get the encoded string: `"ab121p"`.

Given two encoded strings `s1` and `s2`, consisting of lowercase English letters and digits `1-9` (inclusive), return `true` *if there exists an original string that could be encoded as **both*** `s1` *and* `s2`*. Otherwise, return* `false`.

**Note**: The test cases are generated such that the number of consecutive digits in `s1` and `s2` does not exceed `3`.

**Example 1:**

```
Input: s1 = "internationalization", s2 = "i18n"
Output: true
Explanation: It is possible that "internationalization" was the original string.
- "internationalization"
  -> Split:       ["internationalization"]
  -> Do not replace any element
  -> Concatenate:  "internationalization", which is s1.
- "internationalization"
  -> Split:       ["i", "nternationalizatio", "n"]
  -> Replace:     ["i", "18",                 "n"]
  -> Concatenate:  "i18n", which is s2

```

**Example 2:**

```
Input: s1 = "l123e", s2 = "44"
Output: true
Explanation: It is possible that "leetcode" was the original string.
- "leetcode"
  -> Split:      ["l", "e", "et", "cod", "e"]
  -> Replace:    ["l", "1", "2",  "3",   "e"]
  -> Concatenate: "l123e", which is s1.
- "leetcode"
  -> Split:      ["leet", "code"]
  -> Replace:    ["4",    "4"]
  -> Concatenate: "44", which is s2.

```

**Example 3:**

```
Input: s1 = "a5b", s2 = "c5b"
Output: false
Explanation: It is impossible.
- The original string encoded as s1 must start with the letter 'a'.
- The original string encoded as s2 must start with the letter 'c'.

```

**Constraints:**

- `1 <= s1.length, s2.length <= 40`
- `s1` and `s2` consist of digits `1-9` (inclusive), and lowercase English letters only.
- The number of consecutive digits in `s1` and `s2` does not exceed `3`.

comment: why 3rd degree is 2000?

999a999?

when "a" is successfully matched between the two strings, the diff will reset to zero.
when "a" fails to match, the function will prune this branch of the search and return.

```cpp
//cp from votrubac

class Solution {
public:
    bool possiblyEquals(string s1, string s2) {
        return helper(s1,s2,0,0,0);
    }

    /*
    i in s1, j in s2, diff=*1-*2 till cur idx
    diff -999~999
    dp[i][j][d]:
        for s1[i~back], s2[j~back], left side ending wildcards -*1+*2==d, if possible equal or not
    dp[i][j][d]=
        if only one out of bound, return false;
        if both out of bound, return diff==0
        if s1[i] is digit, 
            get the possible num starting from i, move i to nxt idx of ending of num
            for all possible cnt of *, diff-=cnt
                f(i', j, d')
        if s2[j] is digit, 
            get the possible num starting from j, move j to nxt idx of ending of num
                f(i, j', d')
        if both letter
            if diff < 0: more *1, use *1 to match j -> f(i,j+1,d++);
            if diff > 0: more *2, use *2 to match i -> f(i+1,j,d--);
            if *1==*2, f(i+1,j+1,0)
    init:
        i=m, j=n, d=0 => true
        other false
    return:
        dp[0][0][0];
    */
    int M=1000;//offset
    bool visited[41][41][2000] = {};

    bool helper(string &s1, string &s2, int i = 0, int j = 0, int diff = 0) {
        auto processDigits = [&](const string &s, int &p, int sign) {
            //get the number, move the idx to nxt of the number end
            //for each * cnt from the number, match with another str
            for (int val = 0; p < s.size() && isdigit(s[p]);) {
                val = val * 10 + (s[p++] - '0');
                if (helper(s1, s2, i, j, diff + val * sign))
                    return true;
            }
            return false;
        };
        if (i == s1.size() && j == s2.size())
            return diff == 0;
        if (!visited[i][j][M + diff]) {
            visited[i][j][M + diff] = true;
            //i is digit
            if (i < s1.size() && isdigit(s1[i]))
                return processDigits(s1, i, -1);
            //j is digit
            if (j < s2.size() && isdigit(s2[j]))
                return processDigits(s2, j, 1);
            //both letters or end -> *2 more
            if (diff > 0)
                return i < s1.size() && helper(s1, s2, i + 1, j, diff - 1);
            //both letters or end -> *1 more
            if (diff < 0)
                return j < s2.size() && helper(s1, s2, i, j + 1, diff + 1);
            //both letters or end -> *1==*2
            return i < s1.size() && j < s2.size() && s1[i] == s2[j] && helper(s1, s2, i + 1, j + 1, 0);
        }
        return false;
    }   
};

/*
len: 1~40
digit: 1~9
letters: 26
num.len <=3
================================
idx i
idx j
number of * chars
================================
cases:
1. i or j points to a digit
    get all adj digits (up to 3), and generate possible numbers
        at most 999
    advance the pointer and adjust diff, continue recurr
2. i and j points to a letter/end of str
    diff!=0
        match one * with a letter in another str
    diff==0
        match two letters
*/
```