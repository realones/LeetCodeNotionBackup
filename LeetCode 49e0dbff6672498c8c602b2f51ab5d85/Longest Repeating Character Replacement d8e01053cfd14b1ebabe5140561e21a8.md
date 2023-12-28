# Longest Repeating Character Replacement

#: 424
Difficult: Medium
Tags: 2pt_SameDir_maxWin_chkValidBfrR, Hash, string
link: https://leetcode.com/problems/longest-repeating-character-replacement/description/
Priority: Medium
Status: No Idea At First
Created time: December 8, 2023 6:43 PM
Last edited time: December 8, 2023 10:16 PM
source: facebookFreq

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return *the length of the longest substring containing the same letter you can get after performing the above operations*.d

**Example 1:**

```
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.

```

**Example 2:**

```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There may exists other ways to achieve this answer too.
```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of only uppercase English letters.
- `0 <= k <= s.length`

comment: figure out the solution by myself but takes some time

```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        vector<int> counts(26, 0);
        int start = 0;
        int maxCharCount = 0;
        int n = s.length();
        int result = 0;
        for(int end = 0; end < n; end++){
            counts[s[end]-'A']++;
            if(maxCharCount < counts[s[end]-'A']){
                maxCharCount = counts[s[end]-'A'];
            }
            while(end-start-maxCharCount+1 > k){
                counts[s[start]-'A']--;
                start++;
                for(int i = 0; i < 26; i++){
                    if(maxCharCount < counts[i]){
                        maxCharCount = counts[i];
                    }
                }
            }
            result = max(result, end-start+1);
        }
        return result;
    }
};

/*
op:
    choose any char in s, change to another char
op times:
    <=k
return:
    max len of [same letter] substr from above ops
================================
s:
    len: 1~1e5
    val: upper case: 26
k: 0~s.len
================================
solution 1:
dp[i][k][c]: 
    idx i, change k times, char is c, the max substr len from somewhere to i

solution 2:
sliding win/ 2pt:
    find the max sliding window that contains at most K other non-popular chars 
validation:
    contains at most K other non-popular chars:
        mp[char - cnt], 26 -> cnt
        diff = maxCnt - sumOfOtherCnts
*/
```