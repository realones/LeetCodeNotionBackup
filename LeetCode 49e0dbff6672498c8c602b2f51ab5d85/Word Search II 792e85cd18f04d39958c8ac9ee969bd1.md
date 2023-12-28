# Word Search II

#: 212
Difficult: Hard
Tags: BackTracking, DFS, Matrix, Trie, search
link: https://leetcode.com/problems/word-search-ii/
Priority: High
Status: HardToImplement
Created time: August 28, 2022 2:37 AM
Last edited time: October 22, 2023 7:19 PM
practicedTimes: 1
source: LC_Top_Interview_150, jiuzhang, 算高UnionFind&Trie
related: Design Add and Search Words Data Structure (Design%20Add%20and%20Search%20Words%20Data%20Structure%20a7e7faa5cea04664926a7edc19fa9c6f.md)

Given an `m x n` `board` of characters and a list of strings `words`, return *all words on the board*.

Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/11/07/search1.jpg](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

```
Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]

```

**Example 2:**

![https://assets.leetcode.com/uploads/2020/11/07/search2.jpg](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)

```
Input: board = [["a","b"],["c","d"]], words = ["abcb"]
Output: []

```

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 12`
- `board[i][j]` is a lowercase English letter.
- `1 <= words.length <= 3e4`
- `1 <= words[i].length <= 10`
- `words[i]` consists of lowercase English letters.
- All the strings of `words` are unique.

● Hash vs Trie做这道题目的区别

● 把谁建成Trie树?

解题思路:

- 把字典建成Trie树。
- 用dfs的方法遍历矩阵，同时在Trie上搜索前缀是否存在。
- 查询所有Trie里面有可能出现的字符。

```cpp
class Node {
public:
    vector<Node*> nexts;
    bool end;
    string word;
    
    Node(){
        nexts.resize(26,NULL);
        end=false;
        word="";
    }
};

class Trie {
public:
    Node* root;
    
    Trie(){
        root=new Node();
    }
    
    void insert(string word){
        Node *cur=root;
        for(auto c: word){
            if(!cur->nexts[c-'a']){cur->nexts[c-'a']=new Node();}
            cur=cur->nexts[c-'a'];
        }
        cur->end=true;
        cur->word=word;
    }
};

class Solution {
public:
    vector<string> findWords(vector<vector<char>>& matrix, vector<string>& words) {
        int m=matrix.size(); if(m==0) {return {};}
        int n=matrix[0].size(); if(n==0) {return {};}
        if(words.size()==0) return {};
        
        vector<string> res;
        //build trie
        Trie *trie = new Trie();
        for(auto word: words){
            trie->insert(word);
        }
        Node* root = trie->root;
        //for each element in matrix, dfs on cur node
        vector<vector<int>> visited(m,vector<int>(n,0));//can improve to use matrix directly
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                dfs(res, visited, matrix, i, j, root);
            }
        }
        
        return res;
    }
    
    vector<int> d{0,1,0,-1,0};
    void dfs(vector<string>& res, vector<vector<int>>& visited, vector<vector<char>>& matrix, int x, int y, Node* cur){
        //end
        //if visited
        if(visited[x][y]) return;
        //if not match
        char c=matrix[x][y];
        int nxtIdx=c-'a';
        Node* nxt=cur->nexts[nxtIdx];
        if(!nxt) return;
        //if find
        //check nxt but not cur, if check cur, then last dfs is checking empty string with nx,ny which might be visited
        if(nxt->end){
            if(nxt->word!=""){
                res.push_back(nxt->word);
                nxt->word="";//dedup
            }
            //return; //don't return here
        }
        
        //process
        int m=matrix.size();
        int n=matrix[0].size();
        //backTracking visited
        visited[x][y]=true;
        
        //for each neighbor
        for(int k=0;k<4;k++){
            int nx=x+d[k];
            int ny=y+d[k+1];
            if(nx<0 || nx>m-1 || ny<0 || ny>n-1) continue;
            dfs(res,visited,matrix,nx,ny,nxt);
        }
        
        visited[x][y]=false;
    }
    
};
```