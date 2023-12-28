# Verifying an Alien Dictionary

#: 953
Difficult: Easy
Tags: Array, Hash, string
link: https://leetcode.com/problems/verifying-an-alien-dictionary/
Priority: Low
Created time: December 3, 2023 7:14 PM
Last edited time: December 3, 2023 7:14 PM
source: facebookFreq

In an alien language, surprisingly, they also use English lowercase letters, but possibly in a different `order`. The `order` of the alphabet is some permutation of lowercase letters.

Given a sequence of `words` written in the alien language, and the `order` of the alphabet, return `true` if and only if the given `words` are sorted lexicographically in this alien language.

**Example 1:**

```
Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
Output: true
Explanation:As 'h' comes before 'l' in this language, then the sequence is sorted.

```

**Example 2:**

```
Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
Output: false
Explanation:As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.

```

**Example 3:**

```
Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
Output: false
Explanation:The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character (More info).

```

**Constraints:**

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 20`
- `order.length == 26`
- All characters in `words[i]` and `order` are English lowercase letters.

```cpp
class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) {
        unordered_map<char,int> mp;
        for(int i=0;i<order.size();i++){
            mp[order[i]]=i;
        }
        //func
        auto smallerEqual = [&](string& w1, string& w2)->bool{
            int len=min(w1.size(), w2.size());
            for(int i=0; i<len; i++){
                if(mp[w1[i]]<mp[w2[i]]) return true;
                else if(mp[w1[i]]==mp[w2[i]]) continue;
                else return false;
            }
            return w1.size() <= w2.size();
        };

        for(int i=1;i<words.size();i++){
            string& w1=words[i-1];
            string& w2=words[i];
            if(!smallerEqual(w1,w2)) return false;
        }
        return true;
    }

};
```