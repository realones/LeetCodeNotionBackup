# Smallest Sufficient Team

#: 1125
Difficult: Hard
Tags: BitMask, dp-backpack, dp-bitMask
link: https://leetcode.com/problems/smallest-sufficient-team/
Priority: High
Status: Dig More
Created time: January 12, 2023 6:37 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, leetcode_daily, wp动归套路

In a project, you have a list of required skills `req_skills`, and a list of people. The `ith` person `people[i]` contains a list of skills that the person has.

Consider a sufficient team: a set of people such that for every required skill in `req_skills`, there is at least one person in the team who has that skill. We can represent these teams by the index of each person.

- For example, `team = [0, 1, 3]` represents the people with skills `people[0]`, `people[1]`, and `people[3]`.

Return *any sufficient team of the smallest possible size, represented by the index of each person*. You may return the answer in **any order**.

It is **guaranteed** an answer exists.

**Example 1:**

```
Input: req_skills = ["java","nodejs","reactjs"], people = [["java"],["nodejs"],["nodejs","reactjs"]]
Output: [0,2]

```

**Example 2:**

```
Input: req_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = [["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]
Output: [1,2]

```

**Constraints:**

- `1 <= req_skills.length <= 16`
- `1 <= req_skills[i].length <= 16`
- `req_skills[i]` consists of lowercase English letters.
- All the strings of `req_skills` are **unique**.
- `1 <= people.length <= 60`
- `0 <= people[i].length <= 16`
- `1 <= people[i][j].length <= 16`
- `people[i][j]` consists of lowercase English letters.
- All the strings of `people[i]` are **unique**.
- Every skill in `people[i]` is a skill in `req_skills`.
- It is guaranteed a sufficient team exists.

Solution 1 - 1d dp:

```cpp
class Solution {
public:
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
        //states
        int n=req_skills.size();
        int N=1<<n;
        //skill -> bit
        unordered_map<string,int> mp;//skill_bit
        for(int i=0; i<n; i++){
            mp[req_skills[i]]=i;
        }
        //people to skillbits
        vector<int> p2s(people.size());
        for(int i=0;i<people.size();i++){
            for(auto& s: people[i]){
                p2s[i]+=1<<mp[s];
            }
        }
        //dp process
        vector<int> dp(N,INT_MAX);
        vector<pair<int,int>> parent(N,{-1,-1});//parent[state]={preState,peopleIdx}
        dp[0]=0;
        for(int i=0;i<p2s.size();i++){
            for(int s=0;s<N;s++){
                if(dp[s]==INT_MAX) continue;
                //not take -> nothing happens
                //take
                int nxtS=s|p2s[i];
                if(dp[nxtS]>dp[s]+1){
                    dp[nxtS]=dp[s]+1;
                    parent[nxtS]={s,i};
                }
            }
        }

        vector<int> res;
        int s=N-1;
        while(parent[s].first!=-1){
            res.push_back(parent[s].second);
            s=parent[s].first;
        }
        return res;
    }
};

/*
req_skills: len 1~16
people: len: 1~60

brute force: find all combination of people: 2^60, TLE

dp to improve:
bitmask state: selected skills

dp[state] : min number of people required

T: 2^16 * 60 = ~4e6
backpack i->i+1
for each people: 60
    for each state: 2^16, if already has valid value
        notTake: nothing happens
        take:
            dp[nxtState] min= dp[state]+1

dp[state]

init: 0000000=0;
return: 1111111=?

*/
```

Solution 2 - 2d dp:

```jsx
/*
req_skills: skill1, skill2, 3,4,5...
people 1: skill1, skill2, ...
people 2: skill2, skill4, ...
smallest group of people have all req_skills ability.

path compression
state 10110101 + some people -> new_state 11110111

return state 1111111

dp people state = min step
init: dp 0 0 = 0, other INT_MAX

dp people state = 
    for each people
        for each state
            dp people state min= 
                not use: dp people-1 state
                use:     dp people-1 state-people
        
return dp last people, 11111state

*/
class Solution {
public:
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
        int n=req_skills.size();
        int N=1<<n;
        
        unordered_map<string,int> mp;
        for(int i=0;i<n;i++){
            mp[req_skills[i]]=i;
        }
        
        vector<int> p2s(people.size());
        for(int i=0;i<people.size();i++){
            for(auto s: people[i]){
                int si=mp[s];
                p2s[i]+=(1<<si);
            }
        }
        
        
        vector<vector<int>> dp(people.size()+1,vector<int>(N,INT_MAX));
        vector<vector<vector<int>>> saves(people.size()+1,vector<vector<int>>(N,vector<int>()));
        dp[0][0]=0;
        
        for(int i=0; i<people.size(); i++){
            for(int s=0;s<N;s++){
                if(dp[i+1][s]>dp[i][s]){
                    dp[i+1][s]=dp[i][s];
                    saves[i+1][s]=saves[i][s];
                }
                
                int newS=s|p2s[i];
                int tmp = dp[i][s]==INT_MAX? INT_MAX : (dp[i][s]+1);
                if(dp[i+1][newS]>tmp){
                    dp[i+1][newS]=tmp,
                    saves[i+1][newS]=saves[i][s];
                    saves[i+1][newS].push_back(i);
                }
            }
        }
        
        return saves.back().back();
    }
};
```