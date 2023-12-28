# Longest String Chain

#: 1048
Difficult: Medium
Tags: DP
link: https://leetcode.com/problems/longest-string-chain/
Priority: Medium
Status: Dig More, More Solution
Created time: October 24, 2022 2:21 AM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq
related: Longest Increasing Subsequence (Longest%20Increasing%20Subsequence%20dbe9919e92d04b90928f4e005fc84e25.md)

You are given an array of `words` where each word consists of lowercase English letters.

`wordA` is a **predecessor** of `wordB` if and only if we can insert **exactly one** letter anywhere in `wordA` **without changing the order of the other characters** to make it equal to `wordB`.

- For example, `"abc"` is a **predecessor** of `"abac"`, while `"cba"` is not a **predecessor** of `"bcad"`.

A **word chain** **is a sequence of words `[word1, word2, ..., wordk]` with `k >= 1`, where `word1` is a **predecessor** of `word2`, `word2` is a **predecessor** of `word3`, and so on. A single word is trivially a **word chain** with `k == 1`.

Return *the **length** of the **longest possible word chain** with words chosen from the given list of* `words`.

**Example 1:**

```
Input: words = ["a","b","ba","bca","bda","bdca"]
Output: 4
Explanation: One of the longest word chains is ["a","ba","bda","bdca"].

```

**Example 2:**

```
Input: words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
Output: 5
Explanation: All the words can be put in a word chain ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].

```

**Example 3:**

```
Input: words = ["abcd","dbqca"]
Output: 1
Explanation: The trivial word chain ["abcd"] is one of the longest word chains.
["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.

```

**Constraints:**

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 16`
- `words[i]` only consists of lowercase English letters.

todo: [https://leetcode.com/problems/longest-string-chain/discuss/2153007/C%2B%2BPython-Simple-Solution-w-Explanation-or-DP](https://leetcode.com/problems/longest-string-chain/discuss/2153007/C%2B%2BPython-Simple-Solution-w-Explanation-or-DP)

comment: i figure out “Solution; 一层层wordSz遍历” first, this solution is pretty interesting

Solution LIS:

```cpp
bool cmp(const string &x, const string &y) {
    return x.size()<y.size();
}

class Solution {
public:
    int longestStrChain(vector<string>& words) {
        sort(words.begin(), words.end(), cmp);
        int n=words.size();
        int res=0;
        vector<int> dp(n,1);
        for(int i=0; i<n; i++){
            for(int k=0; k<i; k++){
                if(isChain(words[k],words[i])) dp[i]=max(dp[i], dp[k]+1);
            }
            res=max(res,dp[i]);
        }
        return res;
    }

    bool isChain(string& s1, string& s2){
        if(s1.size()+1!=s2.size()) return false;
        int i=0,j=0;
        for(; i<s1.size()&&j<s2.size(); i++,j++){
            if(s1[i]==s2[j]) continue;
            else i--;
        }
        return i==s1.size() && (j==s2.size()||j==s2.size()-1);
    }
};

/*
XXXXX
XXXAXX
*/

//LIS
```

Solution; 一层层wordSz遍历

```cpp
class Solution {
public:
    int longestStrChain(vector<string>& words) {
        //wSz - {w - chainLen}, can be improved by vec[16]
        map<int,vector<pair<string,int>>> mp;
        for(auto w: words){
            mp[w.size()].emplace_back(w,1);
        }
        
        int res=1;
        for(auto& [wSz, vec]: mp){//for sz 2...16
            if(wSz==mp.begin()->first) continue;
            
            for(auto& [w, chainLen]: vec){//for each w
                for(auto& [preW, preChainLen]: mp[wSz-1]){ //for each pre w with sz-1
                    if(canChain(preW,w)){
                        chainLen=max(chainLen,preChainLen+1);
                        res=max(res,chainLen);
                    }
                }
            }
        }
        return res;
    }
    
    bool canChain(string w1, string w2){
        int diff=0;
        int i1=0,i2=0;
        for(;i1<w1.size()&&i2<w2.size();i1++,i2++){
            if(w1[i1]!=w2[i2]){
                i1--;
                diff++;
            }
        }
        return diff==1 || diff==0 && i2==w2.size()-1;
    }
};

/*
size 1 {word-chainlen}
size 2
size 3
...
size 16

for sz 2 -- 16
    for word from sz-1
        if can chain
        cur max= pre+1 (cur init= 1)
        res max= cur
*/
```