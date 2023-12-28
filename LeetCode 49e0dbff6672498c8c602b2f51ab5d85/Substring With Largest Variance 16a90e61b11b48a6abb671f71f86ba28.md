# Substring With Largest Variance

#: 2272
Difficult: Hard
Tags: Array, DP, Kadane
link: https://leetcode.com/problems/substring-with-largest-variance/
Priority: High
Status: More Solution, No Idea At First
Created time: July 10, 2023 12:17 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

The **variance** of a string is defined as the largest difference between the number of occurrences of **any** `2` characters present in the string. Note the two characters may or may not be the same.

Given a string `s` consisting of lowercase English letters only, return *the **largest variance** possible among all **substrings** of* `s`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

```
Input: s = "aababbb"
Output: 3
Explanation:
All possible variances along with their respective substrings are listed below:
- Variance 0 for substrings "a", "aa", "ab", "abab", "aababb", "ba", "b", "bb", and "bbb".
- Variance 1 for substrings "aab", "aba", "abb", "aabab", "ababb", "aababbb", and "bab".
- Variance 2 for substrings "aaba", "ababbb", "abbb", and "babb".
- Variance 3 for substring "babbb".
Since the largest possible variance is 3, we return it.

```

**Example 2:**

```
Input: s = "abcde"
Output: 0
Explanation:
No letter occurs more than once in s, so the variance of every substring is 0.

```

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of lowercase English letters.

**Solution**:

from answer

hints:

1. Think about how to solve the problem if the string had only two distinct characters.
2. If we replace all occurrences of the first character by +1 and those of the second character by -1, can we efficiently calculate the largest possible variance of a string with only two distinct characters?
3. Now, try finding the optimal answer by taking all possible pairs of characters into consideration.

```cpp
class Solution {
public:
    int largestVariance(string s) {
        //calcu freq
        unordered_map<char,int> freq;
        for(char c: s) freq[c]++; 

        //process       
        int res=0;
        for(char minc='a'; minc<='z'; minc++){
            for(char maxc='a'; maxc<='z'; maxc++){
                if(minc==maxc || !freq[minc] || !freq[maxc]) continue;
                //contribution
                int tmp=0;//maxCnt-minCnt for cur minc and maxc
                int minCnt=0, maxCnt=0;
                int minRest=freq[minc];//remaining minc cnt

                //kadane
                for(char c: s){
                    if(c==minc) minCnt++, minRest--;
                    else if(c==maxc) maxCnt++;
                    else continue;
                    // We can discard the previous string if there is at least one remaining minor
                    if(minCnt>0 && maxCnt>0) tmp=max(tmp,maxCnt-minCnt);
                    //update when valid substr
                    if(minCnt>maxCnt && minRest>0) minCnt=0, maxCnt=0;
                }
                res=max(res,tmp);
            }
        }
        return res;
    }
};

/*
for any substr
find one with max diff of char_freq, return the diff

n:1~1e4
O(k*n)

================================
sub questions:
1. give s, how to find max diff of char_freq: mp[char]=cnt, diff = max_cnt - min_cnt

substr: sliding window

greedy: 
    inc most freq one, add r
    dec least freq one till 1, trim l

*/
```