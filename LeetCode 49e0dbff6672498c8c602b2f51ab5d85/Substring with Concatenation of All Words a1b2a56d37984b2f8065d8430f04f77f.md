# Substring with Concatenation of All Words

#: 30
Difficult: Hard
Tags: 2pt_SameDir_sameSpeed, Hash, Sliding Window, string
link: https://leetcode.com/problems/substring-with-concatenation-of-all-words/
Priority: High
Status: Dig More, No Idea At First, toSummarize
Created time: May 12, 2023 2:40 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150, wisdompeak

You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.

A **concatenated substring** in `s` is a substring that contains all the strings of any permutation of `words` concatenated.

- For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated substring because it is not the concatenation of any permutation of `words`.

Return *the starting indices of all the concatenated substrings in* `s`. You can return the answer in **any order**.

**Example 1:**

```
Input: s = "barfoothefoobarman", words = ["foo","bar"]
Output: [0,9]
Explanation: Since words.length == 2 and words[i].length == 3, the concatenated substring has to be of length 6.
The substring starting at 0 is "barfoo". It is the concatenation of ["bar","foo"] which is a permutation of words.
The substring starting at 9 is "foobar". It is the concatenation of ["foo","bar"] which is a permutation of words.
The output order does not matter. Returning [9,0] is fine too.

```

**Example 2:**

```
Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
Output: []
Explanation: Since words.length == 4 and words[i].length == 4, the concatenated substring has to be of length 16.
There is no substring of length 16 is s that is equal to the concatenation of any permutation of words.
We return an empty array.

```

**Example 3:**

```
Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
Output: [6,9,12]
Explanation: Since words.length == 3 and words[i].length == 3, the concatenated substring has to be of length 9.
The substring starting at 6 is "foobarthe". It is the concatenation of ["foo","bar","the"] which is a permutation of words.
The substring starting at 9 is "barthefoo". It is the concatenation of ["bar","the","foo"] which is a permutation of words.
The substring starting at 12 is "thefoobar". It is the concatenation of ["the","foo","bar"] which is a permutation of words.

```

**Constraints:**

- `1 <= s.length <= 104`
- `1 <= words.length <= 5000`
- `1 <= words[i].length <= 30`
- `s` and `words[i]` consist of lowercase English letters.

```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        int n=s.size(); 
        int wLen=words[0].size();
        int totalLen=words[0].size() * words.size();
        
        unordered_map<string,int> mp0;//word--cnt
        for(auto &w: words){
            mp0[w]++;
        }
        int mpSz0=mp0.size();
         
        vector<int> res;
        for(int start=0;start<wLen;start++){
            auto mp=mp0;
            auto mpSz=mpSz0;
            //sliding window
            for(int i=start;i<n;i+=wLen){
                //add cur
                string cur=s.substr(i,wLen);
                if(mp.count(cur)) {
                    mp[cur]--;
                    if(mp[cur]==0) mpSz--;
                }
                if(i+wLen-start>totalLen){
                    string cur=s.substr(i-totalLen,wLen);
                    if(mp.count(cur)) {
                        mp[cur]++;
                        if(mp[cur]==1) mpSz++;
                    }
                }
                if(i+wLen-start>=totalLen){
                    if(mpSz==0) res.push_back(i-totalLen+wLen);
                }
            }
        }
        return res;
    }
};
```