# Sentence Similarity II

#: 737
Difficult: Medium
Tags: Array, BFS, DFS, Graph, Hash, Union Find, string
link: https://leetcode.com/problems/sentence-similarity-ii/description/
Priority: Low
Created time: June 28, 2023 12:27 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

We can represent a sentence as an array of words, for example, the sentence `"I am happy with leetcode"` can be represented as `arr = ["I","am",happy","with","leetcode"]`.

Given two sentences `sentence1` and `sentence2` each represented as a string array and given an array of string pairs `similarPairs` where `similarPairs[i] = [xi, yi]` indicates that the two words `xi` and `yi` are similar.

Return `true` *if `sentence1` and `sentence2` are similar, or* `false` *if they are not similar*.

Two sentences are similar if:

- They have **the same length** (i.e., the same number of words)
- `sentence1[i]` and `sentence2[i]` are similar.

Notice that a word is always similar to itself, also notice that the similarity relation is transitive. For example, if the words `a` and `b` are similar, and the words `b` and `c` are similar, then `a` and `c` are **similar**.

**Example 1:**

```
Input: sentence1 = ["great","acting","skills"], sentence2 = ["fine","drama","talent"], similarPairs = [["great","good"],["fine","good"],["drama","acting"],["skills","talent"]]
Output: true
Explanation: The two sentences have the same length and each word i of sentence1 is also similar to the corresponding word in sentence2.

```

**Example 2:**

```
Input: sentence1 = ["I","love","leetcode"], sentence2 = ["I","love","onepiece"], similarPairs = [["manga","onepiece"],["platform","anime"],["leetcode","platform"],["anime","manga"]]
Output: true
Explanation: "leetcode" --> "platform" --> "anime" --> "manga" --> "onepiece".
Since "leetcode is similar to "onepiece" and the first two words are the same, the two sentences are similar.
```

**Example 3:**

```
Input: sentence1 = ["I","love","leetcode"], sentence2 = ["I","love","onepiece"], similarPairs = [["manga","hunterXhunter"],["platform","anime"],["leetcode","platform"],["anime","manga"]]
Output: false
Explanation: "leetcode" is not similar to "onepiece".

```

**Constraints:**

- `1 <= sentence1.length, sentence2.length <= 1000`
- `1 <= sentence1[i].length, sentence2[i].length <= 20`
- `sentence1[i]` and `sentence2[i]` consist of lower-case and upper-case English letters.
- `0 <= similarPairs.length <= 2000`
- `similarPairs[i].length == 2`
- `1 <= xi.length, yi.length <= 20`
- `xi` and `yi` consist of English letters.

```cpp
class Solution {
public:
    unordered_map<string, string> father;
    
    //with pass compression
    // O(1)
    string find(string a){
        if(!father.count(a)) father[a]=a;
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }
    
    //union - union roots, tree like
    // O(1)
    bool Union(string a, string b) {
        string root_a = find(a);
        string root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }

    bool areSentencesSimilarTwo(vector<string>& sentence1, vector<string>& sentence2, vector<vector<string>>& similarPairs) {
        if(sentence1.size()!=sentence2.size()) return false;
        if(sentence1.size()==0) return true;
        int n=sentence1.size();

        for(auto& simi: similarPairs){
            string x=simi[0], y=simi[1];
            Union(x,y);
        }

        for(int i=0; i<n; i++){
            string& w1=sentence1[i];
            string& w2=sentence2[i];
            if(find(w1)!=find(w2)) return false;
        }
        return true;
    }
};

/*
sen1
sen2

simiar:
- same len
- each idx sen1[i],sen2[i] similar

similarPairs -> similarGroups (since similarity relation is transitive)

then check if s1,s2 similar by check if they are in same group by check if find(s1)==find(s2)

*/
```