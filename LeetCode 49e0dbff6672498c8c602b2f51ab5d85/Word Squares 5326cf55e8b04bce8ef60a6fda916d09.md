# Word Squares

#: 425
Difficult: Hard
Tags: BackTracking, DFS, Permutation, Trie
link: https://leetcode.com/problems/word-squares/
Priority: High
Created time: August 28, 2022 3:28 AM
Last edited time: October 25, 2023 10:20 PM
practicedTimes: 1
source: jiuzhang, 算高UnionFind&Trie

Given an array of **unique** strings `words`, return *all the* **[word squares](https://en.wikipedia.org/wiki/Word_square)** *you can build from* `words`. The same word from `words` can be used **multiple times**. You can return the answer in **any order**.

A sequence of strings forms a valid **word square** if the `kth` row and column read the same string, where `0 <= k < max(numRows, numColumns)`.

- For example, the word sequence `["ball","area","lead","lady"]` forms a word square because each word reads the same both horizontally and vertically.

**Example 1:**

```
Input: words = ["area","lead","wall","lady","ball"]
Output: [["ball","area","lead","lady"],["wall","area","lead","lady"]]
Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).

```

**Example 2:**

```
Input: words = ["abat","baba","atan","atal"]
Output: [["baba","abat","baba","atal"],["baba","abat","baba","atan"]]
Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).

```

**Constraints:**

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 4`
- All `words[i]` have the same length.
- `words[i]` consists of only lowercase English letters.
- All `words[i]` are **unique**.

w/o trie - Time Limit Exceeded

```cpp
class Solution {
public:
    vector<vector<string>> wordSquares(vector<string>& words) {
        if(words.size()==0) return {};
        vector<vector<string>> res;
        vector<string> tmp;
        helper(res, tmp, words);
        return res;
    }
    
    void helper(vector<vector<string>>& res, vector<string>& tmp, vector<string>& words){
        //end: tmp sz == n
        if(tmp.size()==words[0].size()) {
            res.push_back(tmp);
            return;
        }
        //process, for each word, if match prefix, push and update prefix,
        for(auto word: words){
            if(!matchPrefix(word,tmp)) continue;
            tmp.push_back(word);
            helper(res,tmp,words);
            tmp.pop_back();
        }
    }
    
    bool matchPrefix(string word, vector<string>& tmp){
        int sz=tmp.size();
        string prefix="";
        for(auto s: tmp){
            prefix+=s[sz];
        }
        return word.substr(0,prefix.size())==prefix;
    }
};
```

Trie: if决定了前k行，那么k+1行的前k个字母就是确定的，使用trie来减少候选的words

```cpp
class Node {
public:
    bool end;
    string word;
    vector<Node*> nexts;
    Node(){
        end=false;
        word="";
        nexts.resize(26,NULL);
    }
};

class Trie {
public:
    Node* root;
    
    Trie(){
        root=new Node();
    }
    
    void insert(string word){
        Node* cur=root;
        for(auto c: word){
            if(!cur->nexts[c-'a']){cur->nexts[c-'a']=new Node();}
            cur=cur->nexts[c-'a'];
        }
        cur->end=true;
        cur->word=word;
    }
    
    vector<string> getWords(string prefix){
        vector<string> res;
        Node* cur=root;
        for(auto c: prefix){
            if(!cur->nexts[c-'a']){return {};}//not possible
            cur=cur->nexts[c-'a'];
        }
        dfs(res,cur);
        return res;
    }
    
    void dfs(vector<string>& res, Node* cur){
        if(cur->end){res.push_back(cur->word);}
        
        for(auto nxt: cur->nexts){
            if(!nxt) continue;
            dfs(res, nxt);
        }
    }
};

class Solution {
public:
    vector<vector<string>> wordSquares(vector<string>& words) {
        if(words.size()==0) return {};
        //build Trie
        Trie *trie = new Trie();
        for(auto word: words){
            trie->insert(word);
        }
        
        vector<vector<string>> res;
        vector<string> tmp;
        int resSz=words[0].size();
        helper(res, tmp, resSz, trie);
        return res;
    }
    
    void helper(vector<vector<string>>& res, vector<string>& tmp, int resSz, Trie* trie){
        //end: tmp sz == n
        if(tmp.size()==resSz) {
            res.push_back(tmp);
            return;
        }
        //process, for each word, if match prefix, push and update prefix,
        //trie to check prefix, find word with certain prefix only
        //get prefix
        int sz=tmp.size();
        string prefix="";
        for(auto s: tmp){
            prefix+=s[sz];
        }
        
        for(auto word: trie->getWords(prefix)){
            tmp.push_back(word);
            helper(res,tmp,resSz,trie);
            tmp.pop_back();
        }
    }
    
};
```