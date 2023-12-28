# Design Add and Search Words Data Structure

#: 211
Difficult: Medium
Tags: Trie
link: https://leetcode.com/problems/design-add-and-search-words-data-structure/
Priority: High
Status: Dig More
Created time: August 27, 2022 11:45 PM
Last edited time: October 22, 2023 7:16 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算高UnionFind&Trie
related: Implement Magic Dictionary (Implement%20Magic%20Dictionary%20f65b48de8821406bb15ca2acd1fa5388.md), Word Search II (Word%20Search%20II%20792e85cd18f04d39958c8ac9ee969bd1.md)

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

- `WordDictionary()` Initializes the object.
- `void addWord(word)` Adds `word` to the data structure, it can be matched later.
- `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

**Example:**

```
Input
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
Output
[null,null,null,null,false,true,true,true]

Explanation
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True

```

**Constraints:**

- `1 <= word.length <= 25`
- `word` in `addWord` consists of lowercase English letters.
- `word` in `search` consist of `'.'` or lowercase English letters.
- There will be at most `3` dots in `word` for `search` queries.
- At most `104` calls will be made to `addWord` and `search`.

check two solutions

Trie 不是用cur node，而是nexts的位置来储存char:

if last char is ‘.’ , then last search2(curEnd=true, “”) = true

```cpp
class Node {
public:
    bool end;
    vector<Node*> nexts;
    
    Node(){
        nexts.resize(26,NULL);
        end=false;
    }
};
class WordDictionary {
public:
    Node* root;
    
    WordDictionary() {
        root=new Node();
    }
    
    void addWord(string word) {
        Node* cur=root;
        for(auto c: word){
            if(!cur->nexts[c-'a']) cur->nexts[c-'a']=new Node();
            cur=cur->nexts[c-'a'];
        }
        cur->end=true;
    }
    
    bool search(string word) {
        Node* cur=root;
        return search2(cur,word);
    }
    
    bool search2(Node* cur, string word) {
        for(int i=0;i<word.size(); i++){
            if(word[i]=='.'){
                for(auto nxt: cur->nexts){
                    if(nxt){
                        if(search2(nxt, word.substr(i+1))) return true;
                    }
                }
                return false;
            }
            if(!cur->nexts[word[i]-'a']) return false;
            cur=cur->nexts[word[i]-'a'];
        }
        return cur->end;
    }
};

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary* obj = new WordDictionary();
 * obj->addWord(word);
 * bool param_2 = obj->search(word);
 */
```

```cpp
class TrieNode{
public:
    bool isEnd;
    TrieNode *children[26];
    TrieNode() : isEnd(false){
        for (int i = 0; i < 26; i++){children[i] = NULL;}
    }
};

class WordDictionary {
private:
    TrieNode *root;
    
public:
    WordDictionary()
    {
        root = new TrieNode();
    }
    
    // Adds a word into the data structure.
    void addWord(string word) {
        // Write your code here
        TrieNode *cur = root;
        for (int i = 0; i < word.length(); i++){
            int index = word[i] - 'a';
            if (cur->children[index] == NULL){
                cur->children[index] = new TrieNode();
            }

            cur = cur->children[index];
        }

        cur->isEnd = true; 
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    bool search(string word) {
        // Write your code here
        int n = word.length();
        return search(word, n, 0, root);
    }
    
    bool search(string &word, int n, int pos, TrieNode *cur) {
        if (cur == NULL) return false;
        if (pos == n) return cur->isEnd;

        if (word[pos] == '.'){
            for (int i = 0; i < 26; i++){
                if (cur->children[i]){
                    if (search(word, n, pos+1, cur->children[i])){
                        return true;
                    }
                }
            }
        }
        else{
            int index = word[pos] - 'a';
            if (cur->children[index]){
                return search(word, n, pos+1, cur->children[index]);
            }
        }
        return false;
    }
};
```