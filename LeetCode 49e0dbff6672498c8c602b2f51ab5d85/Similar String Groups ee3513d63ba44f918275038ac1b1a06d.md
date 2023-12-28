# Similar String Groups

#: 839
Difficult: Hard
Tags: Array, BFS, DFS, Graph, Union Find, string
link: https://leetcode.com/problems/similar-string-groups/
Priority: Low
Created time: August 24, 2023 1:55 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

Two strings, `X` and `Y`, are considered similar if either they are identical or we can make them equivalent by swapping at most two letters (in distinct positions) within the string `X`.

For example, `"tars"` and `"rats"` are similar (swapping at positions `0` and `2`), and `"rats"` and `"arts"` are similar, but `"star"` is not similar to `"tars"`, `"rats"`, or `"arts"`.

Together, these form two connected groups by similarity: `{"tars", "rats", "arts"}` and `{"star"}`.  Notice that `"tars"` and `"arts"` are in the same group even though they are not similar.  Formally, each group is such that a word is in the group if and only if it is similar to at least one other word in the group.

We are given a list `strs` of strings where every string in `strs` is an anagram of every other string in `strs`. How many groups are there?

**Example 1:**

```
Input: strs = ["tars","rats","arts","star"]
Output: 2

```

**Example 2:**

```
Input: strs = ["omv","ovm"]
Output: 1

```

**Constraints:**

- `1 <= strs.length <= 300`
- `1 <= strs[i].length <= 300`
- `strs[i]` consists of lowercase letters only.
- All words in `strs` have the same length and are anagrams of each other.

comment: 为啥是hard题

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

    int numSimilarGroups(vector<string>& strs) {
        int n=strs.size();
        init(n);
        int cnt=n;
        for(int i=0; i<n; i++){
            for(int j=i+1; j<n; j++){
                if(similar(strs[i],strs[j])){
                    if(Union(i, j)) {cnt--; }
                }
            }
        }
        return cnt;
    }

    bool similar(string& a, string& b){
        int n=a.size();
        int diff=0;
        for(int i=0; i<n; i++){
            if(a[i]!=b[i]) diff++;
        }
        return diff==0 || diff==2;
    }
};
```