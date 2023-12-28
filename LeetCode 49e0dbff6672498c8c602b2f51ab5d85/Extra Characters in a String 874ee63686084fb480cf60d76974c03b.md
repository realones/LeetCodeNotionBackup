# Extra Characters in a String

#: 2707
Difficult: Medium
Tags: Trie, dp-Pyr, dp-TimeSeqFromLastX
link: https://leetcode.com/problems/extra-characters-in-a-string/
Priority: High
Status: Dig More, HardToImplement, More Solution
Created time: May 28, 2023 9:48 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** string `s` and a dictionary of words `dictionary`. You have to break `s` into one or more **non-overlapping** substrings such that each substring is present in `dictionary`. There may be some **extra characters** in `s` which are not present in any of the substrings.

Return *the **minimum** number of extra characters left over if you break up* `s` *optimally.*

**Example 1:**

```
Input: s = "leetscode", dictionary = ["leet","code","leetcode"]
Output: 1
Explanation: We can break s in two substrings: "leet" from index 0 to 3 and "code" from index 5 to 8. There is only 1 unused character (at index 4), so we return 1.

```

**Example 2:**

```
Input: s = "sayhelloworld", dictionary = ["hello","world"]
Output: 3
Explanation: We can break s in two substrings: "hello" from index 3 to 7 and "world" from index 8 to 12. The characters at indices 0, 1, 2 are not used in any substring and thus are considered as extra characters. Hence, we return 3.

```

**Constraints:**

- `1 <= s.length <= 50`
- `1 <= dictionary.length <= 50`
- `1 <= dictionary[i].length <= 50`
- `dictionary[i]` and `s` consists of only lowercase English letters
- `dictionary` contains distinct words

Solution 1:

```cpp
class Solution {
public:
    int minExtraChar(string s, vector<string>& dictionary) {
        unordered_set<string> dict;
        for(auto &s: dictionary){dict.insert(s);}
        
        int n=s.size();
        vector<vector<int>> dp(n,vector<int>(n,INT_MAX));
        
        for(int len=1;len<=n;len++){//len of a brick
            for(int l=0,r;r=l+len-1,r<n;l++){//for each brick
                dp[l][r]=r-l+1;
                if(dict.count(s.substr(l,r-l+1))) {dp[l][r]=0; continue;}
                for(int k=l;k<r;k++){
                    dp[l][r]=min({dp[l][r],
                                  dp[l][k] + dp[k+1][r]});   
                }
            }
        }
        
        return dp[0][n-1];
    }
};

/*
res: 0: all matched, s.size() none matched
f ------------ best way to break s to substrs, less leftover, cnt of leftover

=
f ----
+
f     --------
*/
```

Solution 2:

hard to implement

```cpp
class Solution {
public:
    int minExtraChar(string s, vector<string>& dictionary) {
        int n=s.size();
        unordered_map<int,int> dp;
        dp[-1]=0;
        for(int i=0; i<n; i++){dp[i]=i+1;}
        unordered_set<string> st(dictionary.begin(), dictionary.end());

        for(int i=0; i<n; i++){
            for(int j=0; j<=i; j++){
                int l=dp[j-1];
                string rs=s.substr(j,i-j+1);
                int r=(st.count(rs))?0:i-j+1;
                dp[i] = min(dp[i], l+r);
            }
        }
        return dp[n-1];
    }
};
```