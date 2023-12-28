# [lint] Word Ladder II

#: 121
Difficult: Hard
Tags: BFS, DFS
link: https://www.lintcode.com/problem/121/
Priority: Medium
Status: Next Up
Created time: July 29, 2022 4:03 PM
Last edited time: October 16, 2023 4:58 PM
practicedTimes: 1
source: jiuzhang, 算初DFS

**Description**

Given two words (`start` and `end`), and a dictionary, find all shortest transformation sequence(s) from `start` to `end`.Transformation rule such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

- All words have the same length.
- All words contain only lowercase alphabetic characters.
- At least one solution exists.
- The number of words is less than or equal to 10000
- The word length is less than or equal to 10

**Example**

**Example 1:**

Input:

```
start = "a"
end = "c"
dict =["a","b","c"]

```

Output:

```
[["a","c"]]

```

Explanation:

"a"->"c"

**Example 2:**

Input:

```
start ="hit"
end = "cog"
dict =["hot","dot","dog","lot","log"]

```

Output:

```
[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]

```

Explanation:

1."hit"->"hot"->"dot"->"dog"->"cog"2."hit"->"hot"->"lot"->"log"->"cog"

bfs+dfs(use map instead of set to record depth of each visited element) **other better solutions: 1 only bfs, 2 bfs from both side**

```cpp
class Solution {
public:
    /*
     * @param start: a string
     * @param end: a string
     * @param dict: a set of string
     * @return: a list of lists of string
     */
    vector<vector<string>> findLadders(string &start, string &end, unordered_set<string> &dict) {
        vector<vector<string>> res;
        if(start==end) return vector<vector<string>>{{start}};
        if(dict.empty()) return res;
        
        dict.insert(start);
        dict.insert(end);
        
        unordered_map<string,int> depth;
        //bfs
        bfs(end,start,dict,depth);
        //dfs
        vector<string> tmp;
        tmp.push_back(start);
        dfs(start,end,dict,depth,res,tmp);
        
        return res;
    }
    
    void bfs(string &start, string &end, unordered_set<string> &dict,unordered_map<string,int>& depth){
        queue<string> q;
        q.push(start); depth[start]=0;
        int maxDepth=INT_MAX;
        while(!q.empty()){
            string cur=q.front();q.pop();
            vector<string> nexts=getNexts(cur,dict);
            for(auto next: nexts){
                if(depth.count(next)) continue;
                depth[next]=depth[cur]+1;
                //if(next==end){maxDepth=depth[next];}
                //if(depth[next]>maxDepth) return;
                q.push(next);
            }
        }
    }
    
    void dfs(string &start, string &end, unordered_set<string> &dict,unordered_map<string,int>& depth, vector<vector<string>>& res, vector<string>& tmp){

        if(start==end) res.push_back(tmp);
        
        int nextDepth=depth[start]-1;
        vector<string> nexts=getNexts(start,dict);
        for(auto next:nexts){
            if(depth[next]!=nextDepth) continue;
            tmp.push_back(next);
            dfs(next,end,dict,depth,res,tmp);
            tmp.pop_back();
        }
        
    }
    
    vector<string> getNexts(string s, unordered_set<string> &dict){
        vector<string> res;
        string orig=s;
        for(auto &c:s){
            for(char t='a';t<='z';t++){
                c=t;
                if(s==orig) continue;
                if(dict.count(s)) res.push_back(s);
            }
            s=orig;
        }
        return res;
    }
};
```