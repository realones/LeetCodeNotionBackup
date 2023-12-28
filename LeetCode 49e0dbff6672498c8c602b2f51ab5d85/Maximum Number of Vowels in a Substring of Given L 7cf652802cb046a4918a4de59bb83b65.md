# Maximum Number of Vowels in a Substring of Given Length

#: 1456
Difficult: Medium
Tags: Sliding Window, string
link: https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length
Priority: Pass
Created time: July 7, 2023 4:15 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

Given a string `s` and an integer `k`, return *the maximum number of vowel letters in any substring of* `s` *with length* `k`.

**Vowel letters** in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

**Example 1:**

```
Input: s = "abciiidef", k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowel letters.

```

**Example 2:**

```
Input: s = "aeiou", k = 2
Output: 2
Explanation: Any substring of length 2 contains 2 vowels.

```

**Example 3:**

```
Input: s = "leetcode", k = 3
Output: 2
Explanation: "lee", "eet" and "ode" contain 2 vowels.

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.
- `1 <= k <= s.length`

```cpp
class Solution {
public:
    int maxVowels(string s, int k) {
        int n=s.size();
        int cnt=0, res=0;
        for(int i=0; i<n; i++){
            if(isVowel(s[i])) cnt++;
            if(i>k-1){
                if(isVowel(s[i-k])) cnt--;
            }
            if(i>=k-1) res=max(res,cnt);
        }
        return res;
    }

    bool isVowel(char c){
        return c=='a' || c=='e' || c=='i' || c=='o' || c=='u';
    }
}
```