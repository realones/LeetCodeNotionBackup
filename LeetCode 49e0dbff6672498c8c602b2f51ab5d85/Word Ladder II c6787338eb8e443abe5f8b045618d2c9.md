# Word Ladder II

#: 126
Difficult: Hard
Tags: BFS, BackTracking, DFS, Graph, Hash, string
link: https://leetcode.com/problems/word-ladder-ii/description/
Priority: High
Status: HardToImplement
Created time: June 27, 2023 9:32 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return *all the **shortest transformation sequences** from* `beginWord` *to* `endWord`*, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words* `[beginWord, s1, s2, ..., sk]`.

**Example 1:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
Explanation: There are 2 shortest transformation sequences:
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"

```

**Example 2:**

```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
Output: []
Explanation: The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

```

**Constraints:**

- `1 <= beginWord.length <= 5`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 500`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the words in `wordList` are **unique**.
- The **sum** of all shortest transformation sequences does not exceed `105`.

```cpp
class Solution {
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict;
        for(auto word: wordList){
            dict.insert(word);
        }
        return findLadders2(beginWord, endWord, dict);
    }

    /*
     * @param start: a string
     * @param end: a string
     * @param dict: a set of string
     * @return: a list of lists of string
     */
    vector<vector<string>> findLadders2(string &start, string &end, unordered_set<string> &dict) {
        vector<vector<string>> res;
        if(start==end) return vector<vector<string>>{{start}};
        if(dict.empty()) return res;
        
        dict.insert(start);
        //dict.insert(end);
        
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