# Minimum Genetic Mutation

#: 433
Difficult: Medium
Tags: BFS, Graph, Hash, string
link: https://leetcode.com/problems/minimum-genetic-mutation/
Priority: Low
Status: dup
Created time: June 5, 2023 4:35 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

A gene string can be represented by an 8-character long string, with choices from `'A'`, `'C'`, `'G'`, and `'T'`.

Suppose we need to investigate a mutation from a gene string `startGene` to a gene string `endGene` where one mutation is defined as one single character changed in the gene string.

- For example, `"AACCGGTT" --> "AACCGGTA"` is one mutation.

There is also a gene bank `bank` that records all the valid gene mutations. A gene must be in `bank` to make it a valid gene string.

Given the two gene strings `startGene` and `endGene` and the gene bank `bank`, return *the minimum number of mutations needed to mutate from* `startGene` *to* `endGene`. If there is no such a mutation, return `-1`.

Note that the starting point is assumed to be valid, so it might not be included in the bank.

**Example 1:**

```
Input: startGene = "AACCGGTT", endGene = "AACCGGTA", bank = ["AACCGGTA"]
Output: 1

```

**Example 2:**

```
Input: startGene = "AACCGGTT", endGene = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
Output: 2

```

**Constraints:**

- `0 <= bank.length <= 10`
- `startGene.length == endGene.length == bank[i].length == 8`
- `startGene`, `endGene`, and `bank[i]` consist of only the characters `['A', 'C', 'G', 'T']`.

```cpp
class Solution {
public:
    int minMutation(string startGene, string endGene, vector<string>& bank) {
        if(startGene==endGene) return 0;
        unordered_set<string> st(bank.begin(),bank.end());
        
        int step=0;
        queue<string> q;
        unordered_set<string> visited;
        visited.insert(startGene);
        q.push(startGene);
        while(!q.empty()){
            int qSz=q.size();
            for(int i=0; i<qSz; i++){
                string cur=q.front(); q.pop();
                if(cur==endGene) return step;
                for(string nxt: getNxts(st, cur)){
                    if(visited.count(nxt)) continue;
                    visited.insert(nxt);
                    q.push(nxt);
                }
            }
            step++;
        }
        return -1;
    }
    
    vector<char> gene={'A','C','G','T'};
    vector<string> getNxts(unordered_set<string>& st, string s){
        vector<string> res;
        string origin=s;
        for(auto& c:s){
            s=origin;
            for(auto newC: gene){
                c=newC;
                if(s==origin) continue;
                if(st.count(s)) {
                    res.push_back(s);
                }
            }
        }
        return res;
    }
};
```