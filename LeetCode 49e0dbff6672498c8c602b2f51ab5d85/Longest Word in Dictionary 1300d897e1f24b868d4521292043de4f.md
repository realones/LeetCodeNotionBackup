# Longest Word in Dictionary

#: 720
Difficult: Easy
Tags: Array, Hash, Sort, Trie, string
link: https://leetcode.com/problems/longest-word-in-dictionary/
Priority: Medium
Status: More Solution
Created time: June 27, 2023 9:30 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Given an array of strings `words` representing an English Dictionary, return *the longest word in* `words` *that can be built one character at a time by other words in* `words`.

If there is more than one possible answer, return the longest word with the smallest lexicographical order. If there is no answer, return the empty string.

Note that the word should be built from left to right with each additional character being added to the end of a previous word.

**Example 1:**

```
Input: words = ["w","wo","wor","worl","world"]
Output: "world"
Explanation: The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".

```

**Example 2:**

```
Input: words = ["a","banana","app","appl","ap","apply","apple"]
Output: "apple"
Explanation: Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".

```

**Constraints:**

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 30`
- `words[i]` consists of lowercase English letters.

Solution 1:

```cpp
class Node{
public:
    bool end;
    string s;
    vector<Node*> nexts;
    Node(){
        nexts=vector<Node*>(26,NULL);
        end=false;
    }
};

class Trie {
public:
    Node *root;
    /** Initialize your data structure here. */
    Trie() {
        root=new Node();
        root->end=true;
        root->s="";
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        Node *cur=root;
        for(auto c:word){
            if(cur->nexts[c-'a']==NULL){cur->nexts[c-'a']=new Node();}
            cur=cur->nexts[c-'a'];
        }
        cur->end=true;
        cur->s=word;
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node *cur=root;
        for(auto c:word){
            if(cur->nexts[c-'a']==NULL) return false;
            cur=cur->nexts[c-'a'];
        }
        return (cur->end==true);
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node *cur=root;
        for(auto c:prefix){
            if(cur->nexts[c-'a']==NULL) return false;
            cur=cur->nexts[c-'a'];
        }
        return true;
    }
};

class Solution {
public:
    string longestWord(vector<string>& words) {
        Trie *trie=new Trie();
        for(auto& w: words) trie->insert(w);

        string res="";
        function<void(Node*)> dfs=[&](Node* node){
            if(!node->end) return;
            if(node->s.size()> res.size()){
                res=node->s;
            }
            else if(node->s.size() == res.size() && node->s < res){
                res=node->s;
            }
            for(auto& nxt: node->nexts){
                if(!nxt) continue;
                dfs(nxt);
            }
        };
        dfs(trie->root);

        return res;
    }
};
/*
build trie
dfs each valid node
for continous end=true
maintain the res
*/
```

Solution 2:

```cpp
class Solution {
public:
    string longestWord(vector<string>& words) {
        string res="";
        sort(words.begin(), words.end());
        unordered_set<string> st;
        for(auto w: words){
            if(w.size()==1 || st.count(w.substr(0,w.size()-1))){
                if(w.size()>res.size() || (w.size()==res.size() && w<res)) res=w;
                st.insert(w);
            }
        }
        return res;
    }
}
```