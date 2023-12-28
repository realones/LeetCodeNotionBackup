# Smallest String With Swaps

#: 1202
Difficult: Medium
Tags: BFS, Connected_Component, DFS, Graph, Hash, Union Find, string
link: https://leetcode.com/problems/smallest-string-with-swaps/description/
Priority: Low
Status: No Idea At First
Created time: June 28, 2023 12:23 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given a string `s`, and an array of pairs of indices in the string `pairs` where `pairs[i] = [a, b]` indicates 2 indices(0-indexed) of the string.

You can swap the characters at any pair of indices in the given `pairs` **any number of times**.

Return the lexicographically smallest string that `s` can be changed to after using the swaps.

**Example 1:**

```
Input: s = "dcab", pairs = [[0,3],[1,2]]
Output: "bacd"
Explaination:
Swap s[0] and s[3], s = "bcad"
Swap s[1] and s[2], s = "bacd"

```

**Example 2:**

```
Input: s = "dcab", pairs = [[0,3],[1,2],[0,2]]
Output: "abcd"
Explaination:
Swap s[0] and s[3], s = "bcad"
Swap s[0] and s[2], s = "acbd"
Swap s[1] and s[2], s = "abcd"
```

**Example 3:**

```
Input: s = "cba", pairs = [[0,1],[1,2]]
Output: "abc"
Explaination:
Swap s[0] and s[1], s = "bca"
Swap s[1] and s[2], s = "bac"
Swap s[0] and s[1], s = "abc"

```

**Constraints:**

- `1 <= s.length <= 10^5`
- `0 <= pairs.length <= 10^5`
- `0 <= pairs[i][0], pairs[i][1] < s.length`
- `s` only contains lower case English letters.

comment: 思路想到就好做了，把idx分成不同的group

```cpp
class Solution {
public:
    vector<int> father;
    void init(int n) {
        father.resize(n,0);
        for(int i=0;i<n;i++){
            father[i]=i;//parent is itself
        }
    }

    //with pass compression
    // O(1)
    int find(int a){
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //union - union roots, tree like
    // O(1)
    bool Union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }

    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        int n=s.size();
        init(n);
        for(auto& p: pairs){
            int a=p[0], b=p[1];
            Union(a,b);
        }
        unordered_map<int,set<int>> mp; //groupId - [idx]
        for(int i=0; i<n; i++){
            mp[find(i)].insert(i);
        }

        for(auto& [_,idxs]: mp){
            string tmp;
            for(auto idx: idxs){
                tmp+=s[idx];
            }
            sort(tmp.begin(), tmp.end());;
            int i=0;
            for(auto idx: idxs){
                s[idx]=tmp[i++];
            }
        }
        return s;
    }
};
/*
unionFind

idx groups
0 2 5 8 9
1 3 4
6 7 10

for each group, sort and put int res

*/
```