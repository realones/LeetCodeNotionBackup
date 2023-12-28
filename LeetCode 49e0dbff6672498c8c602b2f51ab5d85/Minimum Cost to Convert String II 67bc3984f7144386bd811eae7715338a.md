# Minimum Cost to Convert String II

#: 2977
Difficult: Hard
Tags: Floyd, Graph, Trie, dp-TimeSeqFromLastX, string
link: https://leetcode.com/problems/minimum-cost-to-convert-string-ii/description/
Priority: High
Status: CalcuComplexity, HardToImplement, No Idea At First, out of scope
Created time: December 25, 2023 5:34 PM
Last edited time: December 25, 2023 6:37 PM
source: leetcode_daily
related: Minimum Cost to Convert String I (Minimum%20Cost%20to%20Convert%20String%20I%20a3701851c67b44e4a67afe392a0b22fa.md)

You are given two **0-indexed** strings `source` and `target`, both of length `n` and consisting of **lowercase** English characters. You are also given two **0-indexed** string arrays `original` and `changed`, and an integer array `cost`, where `cost[i]` represents the cost of converting the string `original[i]` to the string `changed[i]`.

You start with the string `source`. In one operation, you can pick a **substring** `x` from the string, and change it to `y` at a cost of `z` **if** there exists **any** index `j` such that `cost[j] == z`, `original[j] == x`, and `changed[j] == y`. You are allowed to do **any** number of operations, but any pair of operations must satisfy **either** of these two conditions:

- The substrings picked in the operations are `source[a..b]` and `source[c..d]` with either `b < c` **or** `d < a`. In other words, the indices picked in both operations are **disjoint**.
- The substrings picked in the operations are `source[a..b]` and `source[c..d]` with `a == c` **and** `b == d`. In other words, the indices picked in both operations are **identical**.

Return *the **minimum** cost to convert the string* `source` *to the string* `target` *using **any** number of operations*. *If it is impossible to convert* `source` *to* `target`, *return* `-1`.

**Note** that there may exist indices `i`, `j` such that `original[j] == original[i]` and `changed[j] == changed[i]`.

**Example 1:**

```
Input: source = "abcd", target = "acbe", original = ["a","b","c","c","e","d"], changed = ["b","c","b","e","b","e"], cost = [2,5,5,1,2,20]
Output: 28
Explanation: To convert "abcd" to "acbe", do the following operations:
- Change substring source[1..1] from "b" to "c" at a cost of 5.
- Change substring source[2..2] from "c" to "e" at a cost of 1.
- Change substring source[2..2] from "e" to "b" at a cost of 2.
- Change substring source[3..3] from "d" to "e" at a cost of 20.
The total cost incurred is 5 + 1 + 2 + 20 = 28.
It can be shown that this is the minimum possible cost.

```

**Example 2:**

```
Input: source = "abcdefgh", target = "acdeeghh", original = ["bcd","fgh","thh"], changed = ["cde","thh","ghh"], cost = [1,3,5]
Output: 9
Explanation: To convert "abcdefgh" to "acdeeghh", do the following operations:
- Change substring source[1..3] from "bcd" to "cde" at a cost of 1.
- Change substring source[5..7] from "fgh" to "thh" at a cost of 3. We can do this operation because indices [5,7] are disjoint with indices picked in the first operation.
- Change substring source[5..7] from "thh" to "ghh" at a cost of 5. We can do this operation because indices [5,7] are disjoint with indices picked in the first operation, and identical with indices picked in the second operation.
The total cost incurred is 1 + 3 + 5 = 9.
It can be shown that this is the minimum possible cost.

```

**Example 3:**

```
Input: source = "abcdefgh", target = "addddddd", original = ["bcd","defgh"], changed = ["ddd","ddddd"], cost = [100,1578]
Output: -1
Explanation: It is impossible to convert "abcdefgh" to "addddddd".
If you select substring source[1..3] as the first operation to change "abcdefgh" to "adddefgh", you cannot select substring source[3..7] as the second operation because it has a common index, 3, with the first operation.
If you select substring source[3..7] as the first operation to change "abcdefgh" to "abcddddd", you cannot select substring source[1..3] as the second operation because it has a common index, 3, with the first operation.

```

**Constraints:**

- `1 <= source.length == target.length <= 1000`
- `source`, `target` consist only of lowercase English characters.
- `1 <= cost.length == original.length == changed.length <= 100`
- `1 <= original[i].length == changed[i].length <= source.length`
- `original[i]`, `changed[i]` consist only of lowercase English characters.
- `original[i] != changed[i]`
- `1 <= cost[i] <= 106`

Solution TLE:

```cpp
using ll=long long;

class Solution {
public:
    long long minimumCost(string source, string target, vector<string>& original, vector<string>& changed, vector<int>& cost) {
        //离散化discretization
        unordered_set<string> st;
        st.insert(original.begin(), original.end());
        st.insert(changed.begin(), changed.end());
        unordered_map<string ,int> mp;
        int idx=0;
        for(string x: st){
            mp[x]=idx++;
        }
        int V=st.size();
        //floyed
        vector<vector<ll>> d(V,vector<ll>(V,LLONG_MAX/2));
        for(int i=0; i<original.size(); i++)
            d[mp[original[i]]][mp[changed[i]]]=min(d[mp[original[i]]][mp[changed[i]]], (ll)cost[i]);
        for(int i=0; i<V; i++) d[i][i]=0;
        for(int k=0; k<V; k++)
            for(int i=0; i<V; i++)
                for(int j=0; j<V; j++)
                    d[i][j] = min(d[i][j], d[i][k]+d[k][j]);

        //dp
        int n=source.size();
        source = "#" + source;
        target = "#" + target;

        vector<ll> dp(n+1,LLONG_MAX/2);
        dp[0]=0;

        for(int i=1;i<=n;i++){
            if(source[i]==target[i]){//corner case
                dp[i]=dp[i-1];
            }
            for(int j=1;j<=i;j++){
                string s=source.substr(j,i-j+1);
                string t=target.substr(j,i-j+1);
                if(mp.count(s) && mp.count(t) && d[mp[s]][mp[t]]!=LLONG_MAX/2){
                    dp[i] = min(dp[i], dp[j-1]+d[mp[s]][mp[t]]);
                }
            }
        }

        if(dp[n]==LLONG_MAX/2) return -1;
        return dp[n];
    }
};

/*
dp[i]: the min cost to convert s[1:i] to t[1:i]

source X X X X X X X X [X X X X]
                        j     i

target X X X X X X X X [X X X X]
                        j     i

for(int i=1;i<=n;i++)
    for(int j=1;j<=i;j++)
        if(source[j:i] -> target[j:i] exist)
            dp[i] min= dp[j-1]+d[source[j:i] to target[j:i]]
            

d[][]: the min dist(cost) from one str to another

Floyd: O(V^3)
for(int k=1; k<=V; k++)
    for(int i=1; i<=V; i++)
        for(int j=1; j<=V; j++)
            dp[i][j] min= dp[i][k]+dp[k][j]
*/
```

Solution with Trie enhancement:

if i change 

```cpp
ll d[V][V];        
        for (int i=0; i<V; i++)
            for (int j=0; j<V; j++)
            {
                if (i==j)
                    d[i][j] = 0;
                else
                    d[i][j] = LLONG_MAX/2;
            }
```

to

```cpp
vector<vector<ll>> d(V,vector<ll>(V,LLONG_MAX/2));
        for(int i=0; i<V; i++) d[i][i]=0;
```

then TLE

```cpp
using ll=long long;

class Node{
public:
    int idx;
    Node* nexts[26];
    Node(){
        for (int i=0; i<26; i++)
            nexts[i] = NULL;
        idx = -1;
    }
};

class Solution {
public:
    Node* root = new Node();
    long long minimumCost(string source, string target, vector<string>& original, vector<string>& changed, vector<int>& cost) {
        //离散化discretization
        for(auto& s: original) reverse(s.begin(), s.end());
        for(auto& s: changed) reverse(s.begin(), s.end());
        unordered_set<string> st;
        st.insert(original.begin(), original.end());
        st.insert(changed.begin(), changed.end());
        unordered_map<string ,int> mp;
        
        int idx=0;
        for(string x: st){
            mp[x]=idx;
            // reverse(x.begin(), x.end());
            Node *cur=root;
            for(auto c: x){
                if(cur->nexts[c-'a']==NULL){cur->nexts[c-'a']=new Node();}
                cur=cur->nexts[c-'a'];
            }
            cur->idx=idx;
            idx++;
        }

        //floyed
        int V=st.size();
        ll d[V][V];        
        for (int i=0; i<V; i++)
            for (int j=0; j<V; j++)
            {
                if (i==j)
                    d[i][j] = 0;
                else
                    d[i][j] = LLONG_MAX/2;
            }
        for(int i=0; i<original.size(); i++)
            d[mp[original[i]]][mp[changed[i]]]=min(d[mp[original[i]]][mp[changed[i]]], (ll)cost[i]);
        for(int k=0; k<V; k++)
            for(int i=0; i<V; i++)
                for(int j=0; j<V; j++)
                    d[i][j] = min(d[i][j], d[i][k]+d[k][j]);

        //dp
        int n=source.size();
        source = "#" + source;
        target = "#" + target;

        vector<ll> dp(n+1,LLONG_MAX/2);
        dp[0]=0;

        for(int i=1;i<=n;i++){
            if(source[i]==target[i]){//corner case
                dp[i]=dp[i-1];
            }

            Node* node1 = root;
            Node* node2 = root;
            for(int j=i;j>=1;j--){
                //substr is slow
                if(node1->nexts[source[j]-'a']==NULL) break;
                node1=node1->nexts[source[j]-'a'];
                if(node2->nexts[target[j]-'a']==NULL) break;
                node2=node2->nexts[target[j]-'a'];
                //mp.count hash is slow
                int idxS=node1->idx;
                int idxT=node2->idx;
                if(idxS==-1 || idxT==-1) continue;
                dp[i] = min(dp[i], dp[j-1]+d[idxS][idxT]);
            }
        }

        if(dp[n]==LLONG_MAX/2) return -1;
        return dp[n];
    }
};

/*
dp[i]: the min cost to convert s[1:i] to t[1:i]

source X X X X X X X X [X X X X]
                        j     i

target X X X X X X X X [X X X X]
                        j     i

for(int i=1;i<=n;i++)
    for(int j=1;j<=i;j++)
        if(source[j:i] -> target[j:i] exist)
            dp[i] min= dp[j-1]+d[source[j:i] to target[j:i]]
            

d[][]: the min dist(cost) from one str to another

Floyd: O(V^3)
for(int k=1; k<=V; k++)
    for(int i=1; i<=V; i++)
        for(int j=1; j<=V; j++)
            dp[i][j] min= dp[i][k]+dp[k][j]
*/
```