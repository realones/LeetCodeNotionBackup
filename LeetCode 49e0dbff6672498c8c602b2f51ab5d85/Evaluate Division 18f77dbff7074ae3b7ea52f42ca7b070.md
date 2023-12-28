# Evaluate Division

#: 399
Difficult: Medium
Tags: BFS, DFS, Graph, Union Find
link: https://leetcode.com/problems/evaluate-division/
Priority: High
Status: HardToImplement, More Solution
Created time: June 5, 2023 11:02 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75, LC_Top_Interview_150
related: Check for Contradictions in Equations (Check%20for%20Contradictions%20in%20Equations%20fa47b084504e4afa88e6d2439526bc2c.md)

You are given an array of variable pairs `equations` and an array of real numbers `values`, where `equations[i] = [Ai, Bi]` and `values[i]` represent the equation `Ai / Bi = values[i]`. Each `Ai` or `Bi` is a string that represents a single variable.

You are also given some `queries`, where `queries[j] = [Cj, Dj]` represents the `jth` query where you must find the answer for `Cj / Dj = ?`.

Return *the answers to all queries*. If a single answer cannot be determined, return `-1.0`.

**Note:** The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

**Example 1:**

```
Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation:
Given:a / b = 2.0,b / c = 3.0
queries are:a / c = ?,b / a = ?,a / e = ?,a / a = ?,x / x = ?
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]

```

**Example 2:**

```
Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]

```

**Example 3:**

```
Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]

```

**Constraints:**

- `1 <= equations.length <= 20`
- `equations[i].length == 2`
- `1 <= Ai.length, Bi.length <= 5`
- `values.length == equations.length`
- `0.0 < values[i] <= 20.0`
- `1 <= queries.length <= 20`
- `queries[i].length == 2`
- `1 <= Cj.length, Dj.length <= 5`
- `Ai, Bi, Cj, Dj` consist of lower case English letters and digits.

Solution: union find

```cpp
class Solution {
public:
    //init
    unordered_map<string, pair<string,double>> father; //father_rate

    // O(1)
    pair<string,double> find(string a){
        if(!father.count(a)) father[a]={a,1};
        auto [fatherStr, rate] = father[a];
        if (fatherStr == a) return father[a];
        father[a] = find(fatherStr); father[a].second*=rate; //pass compression
        return father[a];
    }

    // O(1)
    // a/b=val -> a=b*val
    bool Union(string a, string b, double val) {
        auto [root_a,rate_a] = find(a);
        auto [root_b,rate_b] = find(b);
        if (root_a != root_b){
            father[root_a] = {root_b,val*rate_b/rate_a};
            return true;
        }
        return false;
    }
    
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        //init
        int n=equations.size();
        for(int i=0; i<n; i++){
            string a=equations[i][0], b=equations[i][1];
            double val=values[i];
            Union(a,b,val);
        }
        //process
        vector<double> res;
        for(auto &q: queries){
            string c=q[0], d=q[1];
            if(!father.count(c) || !father.count(d)) {res.push_back(-1);continue;}
            auto [root_c, rate_c]=find(c);
            auto [root_d, rate_d]=find(d);
            if(root_c!=root_d){res.push_back(-1);continue;}
            res.push_back(rate_c/rate_d);
        }
        return res;
    }
};

/*
equations[i] = [Ai, Bi]
values[i]: Ai / Bi

Ai or Bi: string represents a single variable

queries[j] = [Cj, Dj]

return:
    [Cj / Dj]
    fallback: -1
        
       
directed map
or
all v[0] based, like union find      
*/
```