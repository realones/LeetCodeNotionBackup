# Search Suggestions System

#: 1268
Difficult: Medium
Tags: BS_todo, Trie
link: https://leetcode.com/problems/search-suggestions-system/
Priority: High
Status: Dig More, HardToImplement, More Solution
Created time: June 27, 2023 11:22 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75

You are given an array of strings `products` and a string `searchWord`.

Design a system that suggests at most three product names from `products` after each character of `searchWord` is typed. Suggested products should have common prefix with `searchWord`. If there are more than three products with a common prefix return the three lexicographically minimums products.

Return *a list of lists of the suggested products after each character of* `searchWord` *is typed*.

**Example 1:**

```
Input: products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"
Output: [["mobile","moneypot","monitor"],["mobile","moneypot","monitor"],["mouse","mousepad"],["mouse","mousepad"],["mouse","mousepad"]]
Explanation: products sorted lexicographically = ["mobile","moneypot","monitor","mouse","mousepad"].
After typing m and mo all products match and we show user ["mobile","moneypot","monitor"].
After typing mou, mous and mouse the system suggests ["mouse","mousepad"].

```

**Example 2:**

```
Input: products = ["havana"], searchWord = "havana"
Output: [["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]
Explanation: The only word "havana" will be always suggested while typing the search word.

```

**Constraints:**

- `1 <= products.length <= 1000`
- `1 <= products[i].length <= 3000`
- `1 <= sum(products[i].length) <= 2 * 104`
- All the strings of `products` are **unique**.
- `products[i]` consists of lowercase English letters.
- `1 <= searchWord.length <= 1000`
- `searchWord` consists of lowercase English letters.

Solution:

T/S complexity analysis before coding takes time, need to practice more to comprehend.

```cpp
class Node{
public:
    bool end;
    vector<Node*> nexts;
    string str;
    Node(){
        nexts=vector<Node*>(26,NULL);
        end=false;
        str="";
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
        cur->str=word;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool moveNode(Node*& cur, char c) {
        if(cur->nexts[c-'a']==NULL) return false;
        cur=cur->nexts[c-'a'];
        return true;
    }

    vector<string> getSuggestions(Node* node){
        vector<string> res;
        getSuggestions2(res,node);
        return res;
    }
    void getSuggestions2(vector<string>& res, Node* node){
        if(!node) return;
        if(res.size()>=3) return;
        if(node->end) res.push_back(node->str);
        for(auto nxt: node->nexts){
            if(nxt==NULL) continue;
            getSuggestions2(res,nxt);
        }
    }
};

class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
        //trie: S:1000*3000*(26)
        Trie *trie=new Trie();
        for(auto prod: products) trie->insert(prod);

        vector<vector<string>> res(searchWord.size());
        Node* node=trie->root;
        for(int i=0;i<searchWord.size();i++){//t:1000
            if(!trie->moveNode(node, searchWord[i])) break;
            //t:3000
            vector<string> suggestions=trie->getSuggestions(node);
            res[i]=(suggestions);
        }
        return res;
    }
};
```