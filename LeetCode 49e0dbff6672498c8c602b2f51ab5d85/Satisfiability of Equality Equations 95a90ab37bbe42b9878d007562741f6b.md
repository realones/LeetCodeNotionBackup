# Satisfiability of Equality Equations

#: 990
Difficult: Medium
Tags: Array, Graph, Union Find, string
link: https://leetcode.com/problems/satisfiability-of-equality-equations/description/
Priority: Low
Created time: August 24, 2023 3:29 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given an array of strings `equations` that represent relationships between variables where each string `equations[i]` is of length `4` and takes one of two different forms: `"xi==yi"` or `"xi!=yi"`.Here, `xi` and `yi` are lowercase letters (not necessarily different) that represent one-letter variable names.

Return `true` *if it is possible to assign integers to variable names so as to satisfy all the given equations, or* `false` *otherwise*.

**Example 1:**

```
Input: equations = ["a==b","b!=a"]
Output: false
Explanation: If we assign say, a = 1 and b = 1, then the first equation is satisfied, but not the second.
There is no way to assign the variables to satisfy both equations.

```

**Example 2:**

```
Input: equations = ["b==a","a==b"]
Output: true
Explanation: We could assign a = 1 and b = 1 to satisfy both equations.

```

**Constraints:**

- `1 <= equations.length <= 500`
- `equations[i].length == 4`
- `equations[i][0]` is a lowercase letter.
- `equations[i][1]` is either `'='` or `'!'`.
- `equations[i][2]` is `'='`.
- `equations[i][3]` is a lowercase letter.

```cpp
class Solution {
public:
    unordered_map<char,char> father;

    //with pass compression
    // O(1)
    char find(char a){
        if(!father.count(a)) father[a]=a;
        if (father[a] == a) return a;
        father[a] = find(father[a]); //pass compression
        return father[a];
    }

    //union - union roots, tree like
    // O(1)
    bool Union(char a, char b) {
        char root_a = find(a);
        char root_b = find(b);
        if (root_a != root_b){
            father[root_a] = root_b;
            return true;
        }
        return false;
    }
    
    bool equationsPossible(vector<string>& equations) {
        for(auto& eq: equations){
            char a=eq[0], b=eq[3];
            bool equal=(eq[1]!='!');
            if(equal){
                Union(a,b);
            }
        }
        for(auto& eq: equations){
            char a=eq[0], b=eq[3];
            bool equal=(eq[1]!='!');
            if(!equal){
                if(find(a)==find(b)) return false;
            }
        }
        return true;
    }
};

/*
union find all ==
for each !=, check if have diff root
*/
```