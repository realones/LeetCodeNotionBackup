# Implement Trie (Prefix Tree)

#: 208
Difficult: Medium
Tags: Design, Tree, Trie
link: https://leetcode.com/problems/implement-trie-prefix-tree/
Priority: Medium
Created time: August 27, 2022 11:05 PM
Last edited time: October 22, 2023 3:13 PM
practicedTimes: 1
source: HuaHua, LC_75, LC_Top_Interview_150, jiuzhang, 算高UnionFind&Trie

A **[trie](https://en.wikipedia.org/wiki/Trie)** (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

- `Trie()` Initializes the trie object.
- `void insert(String word)` Inserts the string `word` into the trie.
- `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

**Example 1:**

```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True

```

**Constraints:**

- `1 <= word.length, prefix.length <= 2000`
- `word` and `prefix` consist only of lowercase English letters.
- At most `3 * 104` calls **in total** will be made to `insert`, `search`, and `startsWith`.

```cpp
class Node{
public:
    bool end;
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
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Node *cur=root;
        for(auto c:word){
            if(cur->nexts[c-'a']==NULL){cur->nexts[c-'a']=new Node();}
            cur=cur->nexts[c-'a'];
        }
        cur->end=true;
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

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * bool param_2 = obj.search(word);
 * bool param_3 = obj.startsWith(prefix);
 */
```