# Rearrange String k Distance Apart

#: 358
Difficult: Hard
Tags: Greedy, PQ
link: https://leetcode.com/problems/rearrange-string-k-distance-apart/description/
Priority: High
Status: More Solution, No Idea At First
Created time: September 8, 2023 1:00 AM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily
related: Reorganize String (Reorganize%20String%20495358ec445246e6b47c9c29bda1f242.md)

Given a string `s` and an integer `k`, rearrange `s` such that the same characters are **at least** distance `k` from each other. If it is not possible to rearrange the string, return an empty string `""`.

**Example 1:**

```
Input: s = "aabbcc", k = 3
Output: "abcabc"
Explanation: The same letters are at least a distance of 3 from each other.

```

**Example 2:**

```
Input: s = "aaabc", k = 3
Output: ""
Explanation: It is not possible to rearrange the string.

```

**Example 3:**

```
Input: s = "aaadbbcc", k = 2
Output: "abacabcd"
Explanation: The same letters are at least a distance of 2 from each other.

```

**Constraints:**

- `1 <= s.length <= 3 * 105`
- `s` consists of only lowercase English letters.
- `0 <= k <= s.length`

Solution PQ: put into each K length section

```cpp
class Solution {
public:
    string rearrangeString(string s, int k) {
        if(k==0) return s;
        int n=s.size();
        
        unordered_map<char,int> char_freq;
        for(auto c: s) char_freq[c]++;
        priority_queue<pair<int,char>> pq;
        for(auto [c,f]: char_freq) pq.push({f,c});

        //every time pick k(or last n%k) elements from pq and put into res, sepcial handling for last section which is n%k
        //if not able to pick k(or last n%k) elements(pq.size()<k) -> return ""

        string res(n,'A');
        int remain=n;
        int i=0;
        while(remain>0){
            int take=min(k,remain);
            std::cout << "take" << " -- " << take << std::endl;
            if(pq.size()<take) return "";
            vector<pair<int,char>> toPushBack;
            for(int t=0; t<take; t++){
                auto [f,c] = pq.top(); pq.pop();
                if(f>1) toPushBack.push_back({f-1,c});
                res[i++]=c;
            }
            for(auto p: toPushBack){
                pq.push(p);
            }
            remain-=take;
        }
        return res;
    }
};
```