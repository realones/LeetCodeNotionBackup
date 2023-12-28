# Total Appeal of A String

#: 2262
Difficult: Hard
Tags: DP, Hash, string
link: https://leetcode.com/problems/total-appeal-of-a-string/description/
Priority: Medium
Created time: November 23, 2023 5:26 PM
Last edited time: November 23, 2023 5:27 PM
source: AmazonFreq

The **appeal** of a string is the number of **distinct** characters found in the string.

- For example, the appeal of `"abbca"` is `3` because it has `3` distinct characters: `'a'`, `'b'`, and `'c'`.

Given a string `s`, return *the **total appeal of all of its substrings.***

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

```
Input: s = "abbca"
Output: 28
Explanation: The following are the substrings of "abbca":
- Substrings of length 1: "a", "b", "b", "c", "a" have an appeal of 1, 1, 1, 1, and 1 respectively. The sum is 5.
- Substrings of length 2: "ab", "bb", "bc", "ca" have an appeal of 2, 1, 2, and 2 respectively. The sum is 7.
- Substrings of length 3: "abb", "bbc", "bca" have an appeal of 2, 2, and 3 respectively. The sum is 7.
- Substrings of length 4: "abbc", "bbca" have an appeal of 3 and 3 respectively. The sum is 6.
- Substrings of length 5: "abbca" has an appeal of 3. The sum is 3.
The total sum is 5 + 7 + 7 + 6 + 3 = 28.

```

**Example 2:**

```
Input: s = "code"
Output: 20
Explanation: The following are the substrings of "code":
- Substrings of length 1: "c", "o", "d", "e" have an appeal of 1, 1, 1, and 1 respectively. The sum is 4.
- Substrings of length 2: "co", "od", "de" have an appeal of 2, 2, and 2 respectively. The sum is 6.
- Substrings of length 3: "cod", "ode" have an appeal of 3 and 3 respectively. The sum is 6.
- Substrings of length 4: "code" has an appeal of 4. The sum is 4.
The total sum is 4 + 6 + 6 + 4 = 20.

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.

```cpp
using ll= long long;

class Solution {
public:
    long long appealSum(string s) {
        int n=s.size();
        ll res=0;
        //most recent seen of same char before cur position
        unordered_map<char,int> lastSeen;
        vector<int> mostRecentSeen(n,-1);
        for(int i=0; i<n; i++){
            char c=s[i];
            if(lastSeen.count(c)) mostRecentSeen[i]=lastSeen[c];
            lastSeen[c]=i;
        }
        // for r - 1e5
        vector<ll> dp(n,0);
        for(int r=0;r<n;r++){
            int idx=mostRecentSeen[r];
            if(idx==-1){
                //only care about substr end with r
                dp[r] = (r-1>=0?dp[r-1]:0) + r+1;
            }
            else{
                //only care about substr end with r
                // xxx idx xxr
                // 000  0  111
                dp[r] = (r-1>=0?dp[r-1]:0) + r-idx;
            }
        }
        return accumulate(dp.begin(), dp.end(),0LL);
    }
};

/*
appeal: distinct char cnt in str
give string s
return:
    sum of appeals of all substrs
================================
s
   len: 1~1e5
   val: lowerCase -> 26
================================
solution 1: bruteforce:
    for l, O(1e5)
        for r, O(1e5)
            accumulate appeal O(1e5) -> preprocess to O(1)?
Solution 2:
    for r - 1e5
        if know first position s[r] seen, then
            first part < idx -> pre+=1
            second part >= idx -> pre+=0
    with preprocess of most recent position of seen char, for each char
*/
```