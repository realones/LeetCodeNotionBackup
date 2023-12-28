# Implement Magic Dictionary

#: 676
Difficult: Medium
Tags: DFS, Design, Hash, Trie, string
link: https://leetcode.com/problems/implement-magic-dictionary/
Priority: High
Status: HardToImplement, More Solution
Created time: September 11, 2023 9:41 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Design Add and Search Words Data Structure (Design%20Add%20and%20Search%20Words%20Data%20Structure%20a7e7faa5cea04664926a7edc19fa9c6f.md)

Design a data structure that is initialized with a list of **different** words. Provided a string, you should determine if you can change exactly one character in this string to match any word in the data structure.

Implement the `MagicDictionary` class:

- `MagicDictionary()` Initializes the object.
- `void buildDict(String[] dictionary)` Sets the data structure with an array of distinct strings `dictionary`.
- `bool search(String searchWord)` Returns `true` if you can change **exactly one character** in `searchWord` to match any string in the data structure, otherwise returns `false`.

**Example 1:**

```
Input
["MagicDictionary", "buildDict", "search", "search", "search", "search"]
[[], [["hello", "leetcode"]], ["hello"], ["hhllo"], ["hell"], ["leetcoded"]]
Output
[null, null, false, true, false, false]

Explanation
MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict(["hello", "leetcode"]);
magicDictionary.search("hello"); // return False
magicDictionary.search("hhllo"); // We can change the second 'h' to 'e' to match "hello" so we return True
magicDictionary.search("hell"); // return False
magicDictionary.search("leetcoded"); // return False

```

**Constraints:**

- `1 <= dictionary.length <= 100`
- `1 <= dictionary[i].length <= 100`
- `dictionary[i]` consists of only lower-case English letters.
- All the strings in `dictionary` are **distinct**.
- `1 <= searchWord.length <= 100`
- `searchWord` consists of only lower-case English letters.
- `buildDict` will be called only once before `search`.
- At most `100` calls will be made to `search`.

comment: 

todo: the dfs implementation is not straightforward, need to practice writing pseudo code.

Solution Trie+ dfs

```cpp
class Node{
public:
    bool end;
    vector<Node*> nexts;
    string s;
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
        cur->s=word;
    }

    bool search(string word){
        return search2(root, word, false);
    }

    bool search2(Node* cur, string word, bool flipped) {
        for(int i=0;i<word.size();i++){
            if(!flipped) {
                for(auto nxt: cur->nexts){
                    if(!nxt) continue;
                    if(nxt == cur->nexts[word[i]-'a']) continue;
                    if(search2(nxt, word.substr(i+1), true)) {
                        return true;
                    }
                }
            }
            if(!cur->nexts[word[i]-'a']) return false;
            cur=cur->nexts[word[i]-'a'];
        }
        return (cur->end==true) && flipped==true;
    }
};

class MagicDictionary {
public:
    Trie *trie;
    MagicDictionary() {
        trie=new Trie();
    }
    
    void buildDict(vector<string> dictionary) {
        for(string& s: dictionary){
            trie->insert(s);
        }
    }
    
    bool search(string searchWord) {
        return trie->search(searchWord);
    }
};

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary* obj = new MagicDictionary();
 * obj->buildDict(dictionary);
 * bool param_2 = obj->search(searchWord);
 */
 
 /*
trie
dfs(node, str, ifUsed){
    normal search, if not match, 0->1 if used, dfs to next layer 
        (node->any, str.substr(next idx), 1)

    //ifused - 
        if need to use, 0->1 only
        in the end, check == 1
}
*/
```