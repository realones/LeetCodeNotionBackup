# Check for Contradictions in Equations

#: 2307
Difficult: Hard
Tags: Array, DFS, Graph, Union Find
link: https://leetcode.com/problems/check-for-contradictions-in-equations/
Priority: High
Status: HardToImplement
Created time: November 15, 2023 4:42 PM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq
related: Evaluate Division (Evaluate%20Division%2018f77dbff7074ae3b7ea52f42ca7b070.md)

You are given a 2D array of strings `equations` and an array of real numbers `values`, where `equations[i] = [Ai, Bi]` and `values[i]` means that `Ai / Bi = values[i]`.

Determine if there exists a contradiction in the equations. Return `true` *if there is a contradiction, or* `false` *otherwise*.

**Note**:

- When checking if two numbers are equal, check that their **absolute difference** is less than `105`.
- The testcases are generated such that there are no cases targeting precision, i.e. using `double` is enough to solve the problem.

**Example 1:**

```
Input: equations = [["a","b"],["b","c"],["a","c"]], values = [3,0.5,1.5]
Output: false
Explanation:
The given equations are: a / b = 3, b / c = 0.5, a / c = 1.5
There are no contradictions in the equations. One possible assignment to satisfy all equations is:
a = 3, b = 1 and c = 2.

```

**Example 2:**

```
Input: equations = [["le","et"],["le","code"],["code","et"]], values = [2,5,0.5]
Output: true
Explanation:
The given equations are: le / et = 2, le / code = 5, code / et = 0.5
Based on the first two equations, we get code / et = 0.4.
Since the third equation is code / et = 0.5, we get a contradiction.

```

**Constraints:**

- `1 <= equations.length <= 100`
- `equations[i].length == 2`
- `1 <= Ai.length, Bi.length <= 5`
- `Ai`, `Bi` consist of lowercase English letters.
- `equations.length == values.length`
- `0.0 < values[i] <= 10.0`
- `values[i]` has a maximum of 2 decimal places.

```cpp
class Solution {
public:
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
        else{
            if(abs(rate_a/rate_b - val)<1e-5) return true;
            return false;
        }
    }

    bool checkContradictions(vector<vector<string>>& equations, vector<double>& values) {
        int n=equations.size();
        for(int i=0; i<n; i++){
            string a=equations[i][0], b=equations[i][1]; double v=values[i];// a/b=v
            if(Union(a,b,v)) continue;
            else return true;
        }
        return false;
    }
};

/*
eq: a/b=v
================================
len: 1~100
elem
    len: 1~5
    lowerCase only: 26
val: (0,10.0]
================================
unionFind
*/
```