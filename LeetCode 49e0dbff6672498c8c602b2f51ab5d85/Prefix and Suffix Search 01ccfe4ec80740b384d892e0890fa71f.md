# Prefix and Suffix Search

#: 745
Difficult: Hard
Tags: Design, Hash, Trie, string
link: https://leetcode.com/problems/prefix-and-suffix-search/description/
Priority: High
Status: CalcuComplexity, Dig More, More Solution, No Idea At First, toSummarize
Created time: June 27, 2023 9:29 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Design a special dictionary that searches the words in it by a prefix and a suffix.

Implement the `WordFilter` class:

- `WordFilter(string[] words)` Initializes the object with the `words` in the dictionary.
- `f(string pref, string suff)` Returns *the index of the word in the dictionary,* which has the prefix `pref` and the suffix `suff`. If there is more than one valid index, return **the largest** of them. If there is no such word in the dictionary, return `1`.

**Example 1:**

```
Input
["WordFilter", "f"]
[[["apple"]], ["a", "e"]]
Output
[null, 0]
Explanation
WordFilter wordFilter = new WordFilter(["apple"]);
wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = "e".

```

**Constraints:**

- `1 <= words.length <= 104`
- `1 <= words[i].length <= 7`
- `1 <= pref.length, suff.length <= 7`
- `words[i]`, `pref` and `suff` consist of lowercase English letters only.
- At most `104` calls will be made to the function `f`.

comment: 仔细看看这道题提升复杂度的思路，以及对于pref和suff的处理

Solution - one trie:

```cpp
//used two trie for prefix and postfix, then TLE, then checked leetcode hints
class Node{
public:
    bool end;
    int idx;
    vector<Node*> nexts;
    Node(){
        nexts=vector<Node*>(27,NULL);//a~z + {
        end=false;
        idx=-1;
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
    void insert(string word, int idx) {
        Node *cur=root;
        for(auto c:word){
            if(cur->nexts[c-'a']==NULL){cur->nexts[c-'a']=new Node();}
            cur=cur->nexts[c-'a'];
            cur->idx=max(cur->idx, idx);
        }
        cur->end=true;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    int startsWith(string prefix) {
        Node *cur=root;
        for(auto c:prefix){
            if(cur->nexts[c-'a']==NULL) return -1;
            cur=cur->nexts[c-'a'];
        }
        return cur->idx;
    }
};

class WordFilter {
public:
    Trie* trie;
    
    WordFilter(vector<string>& words) {
        int n=words.size();
        trie=new Trie();
        unordered_map<string,int> mp;//w,idx - dedup
        for(int i=0;i<n;i++) mp[words[i]]=i;

        for(auto& [w,i]: mp){
            for(int sz=1;sz<=w.size();sz++){
                string suff=w.substr(w.size()-sz);
                trie->insert(suff + "{" + w, i);
            }
        }
    }
    
    int f(string pref, string suff) {
        return trie->startsWith(suff+"{"+pref);
    }
};

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter* obj = new WordFilter(words);
 * int param_1 = obj->f(pref,suff);
 */
```

Solution Hash:

7 times slower

```cpp
class WordFilter {
public:
    unordered_map<string, int> mp;
    WordFilter(vector<string>& words) {
        int n=words.size();
        unordered_map<string,int> wordMp;//w_idx
        for(int i=0; i<n; i++) wordMp[words[i]]=i;

        for(auto& [w,i]: wordMp){//1e4
            //7*7=49 combination
            for(int sz1=1;sz1<=w.size();sz1++){
                string pref=w.substr(0,sz1);
                for(int sz2=1;sz2<=w.size();sz2++){
                    string suff=w.substr(w.size()-sz2);
                    mp[pref+"|"+suff]=max(mp[pref+"|"+suff],i);
                }
            }
        }
    }
    
    int f(string pref, string suff) {//1e4
        if(!mp.count(pref+"|"+suff)) return -1;
        return mp[pref+"|"+suff];
    }
};

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter* obj = new WordFilter(words);
 * int param_1 = obj->f(pref,suff);
 */
```

Solution TLE:

```cpp
class Node{
public:
    bool end;
    unordered_set<int> st;
    vector<Node*> nexts;
    Node(){
        nexts=vector<Node*>(26,NULL);
        end=false;
        st={};
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
    void insert(string word, int idx) {
        Node *cur=root;
        for(auto c:word){
            if(cur->nexts[c-'a']==NULL){cur->nexts[c-'a']=new Node();}
            cur=cur->nexts[c-'a'];
            cur->st.insert(idx);
        }
        cur->end=true;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    unordered_set<int> startsWith(string prefix) {
        Node *cur=root;
        for(auto c:prefix){
            if(cur->nexts[c-'a']==NULL) return {};
            cur=cur->nexts[c-'a'];
        }
        return cur->st;
    }
};

class WordFilter {
public:
    Trie* prefixTrie;
    Trie* postTrie;
    
    WordFilter(vector<string>& words) {
        int n=words.size();
        prefixTrie=new Trie();
        postTrie=new Trie();
        for(int i=0; i<n; i++){
            prefixTrie->insert(words[i],i);
            reverse(words[i].begin(), words[i].end());
            postTrie->insert(words[i],i);
        }
    }
    
    int f(string pref, string suff) {
        unordered_set<int> st1=prefixTrie->startsWith(pref);
        reverse(suff.begin(), suff.end());
        unordered_set<int> st2=postTrie->startsWith(suff);
        int res=-1;
        for(auto x: st1){
            if(st2.count(x) && x>res) res=x;
        }
        return res;
    }
};

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter* obj = new WordFilter(words);
 * int param_1 = obj->f(pref,suff);
 */
```