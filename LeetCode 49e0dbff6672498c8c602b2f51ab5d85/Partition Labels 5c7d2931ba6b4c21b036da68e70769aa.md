# Partition Labels

#: 763
Difficult: Medium
Tags: 2pt, Greedy, Hash, Interval, string
link: https://leetcode.com/problems/partition-labels/description/
Priority: Medium
Status: More Solution, toRevisit
Created time: June 27, 2023 9:29 PM
Last edited time: December 3, 2023 7:34 PM
source: HuaHua

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return *a list of integers representing the size of these parts*.

**Example 1:**

```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.

```

**Example 2:**

```
Input: s = "eccbbbbdec"
Output: [10]

```

**Constraints:**

- `1 <= s.length <= 500`
- `s` consists of lowercase English letters.

todo: greedy solution

```cpp
using ii=pair<int,int>;

class Solution {
public:
    vector<int> partitionLabels(string s) {
        int n=s.size();
        unordered_map<char, ii> mp;//c - <first,last>
        for(int i=0; i<n; i++){
            if(!mp.count(s[i])){
                mp[s[i]]={i,i};
            }
            else{
                mp[s[i]].second=i;
            }
        }
        vector<ii> v;
        for(auto [c,p]: mp){
            auto [l,r]=p;
            v.emplace_back(l,r);
        }
        //merge intervals
        sort(v.begin(), v.end());
        vector<ii> merged{{-1,-1}};
        for(auto [l,r]: v){
            if(l > merged.back().second)
                merged.emplace_back(l,r);
            else
                merged.back().second = max(merged.back().second, r);
        }
        //translate
        vector<int> res;
        for(int i=1;i<merged.size();i++){
            auto [l,r] = merged[i];
            res.push_back(r-l+1);
        }
        return res;
    }
};

/*
s: len: 1~500
   val: lowercase only: 26
================================
for each cur, we should know the first seen and last seen
there are multiple intervals, -> interval merging
*/
```