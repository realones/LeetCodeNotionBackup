# Stickers to Spell Word

#: 691
Difficult: Hard
Tags: dp-backpack, dp-bitMask, stateMachine
link: https://leetcode.com/problems/stickers-to-spell-word/
Priority: High
Status: Dig More
Created time: January 10, 2023 3:05 PM
Last edited time: October 12, 2023 2:56 PM
source: wp动归套路

We are given `n` different types of `stickers`. Each sticker has a lowercase English word on it.

You would like to spell out the given string `target` by cutting individual letters from your collection of stickers and rearranging them. You can use each sticker more than once if you want, and you have infinite quantities of each sticker.

Return *the minimum number of stickers that you need to spell out* `target`. If the task is impossible, return `-1`.

**Note:** In all test cases, all words were chosen randomly from the `1000` most common US English words, and `target` was chosen as a concatenation of two random words.

**Example 1:**

```
Input: stickers = ["with","example","science"], target = "thehat"
Output: 3
Explanation:
We can use 2 "with" stickers, and 1 "example" sticker.
After cutting and rearrange the letters of those stickers, we can form the target "thehat".
Also, this is the minimum number of stickers necessary to form the target string.

```

**Example 2:**

```
Input: stickers = ["notice","possible"], target = "basicbasic"
Output: -1
Explanation:
We cannot form the target "basicbasic" from cutting letters from the given stickers.

```

**Constraints:**

- `n == stickers.length`
- `1 <= n <= 50`
- `1 <= stickers[i].length <= 10`
- `1 <= target.length <= 15`
- `stickers[i]` and `target` consist of lowercase English letters.

Solution 1:

bit mask

similar to backpack i→i+1, but state machine idea

```jsx
/*
n=target.size
N=2^n=1<<n

each digit i in n means if target[i] is santisfied 
in total N-1 possibilities (backpack thought)

dp[till ith sticker][state's possible combination] = min sticker cnt required
***!state can only from prev to cur, not from future

solution 1:
for i 1...stickers.size()
    for state combination 0...N-1
        ...
return dp[sticker.sz][N-1]

solution 2;
for state combination 0...N-1
    for i 1...stickers.size()
        dp[state] min= dp[state-sticker] + 1
        
==solution 2;
for state combination 0...N-1
    dp[state] min= dp[state - sticker (for each sticker)]+1
    
solution 2;
for state combination 0...N-1
    ->
    dp[state + sticker (for each sticker)] min= dp[state]+1
return dp[N-1]
        
*/

class Solution {
public:
    int minStickers(vector<string>& stickers, string target) {
        int n=target.size();
        int N=1<<n;
        
        //backpack with path compression
        vector<int> dp(N,INT_MAX);
        dp[0]=0;
        
        for(int state=0;state<N;state++){
            if(dp[state]==INT_MAX) continue;
            
            for(auto sticker: stickers){
                int new_state=nxtState(state, sticker, target);
                dp[new_state]=min(dp[new_state], dp[state]+1);
            }
        }
        
        return dp[N-1]==INT_MAX? -1: dp[N-1];
    }
    
    //state + sticker
    int nxtState(int state, string sticker, string target){
        for(auto c: sticker){
            for(int k=0;k<target.size();k++){
                if(((state>>k)&1)==0 && c==target[k]){//pay attention to (state>>k)&1) paranthesis
                    state+=(1<<k);
                    break;
                }
            }
        }
        return state;
    }
};
```

Solution - Time Limit Exceeded

```jsx
class Solution {
public:
    unordered_map<string,int> dp;
    int minStickers(vector<string>& stickers, string target) {
        
        dp[translate("")]=0;
        
        vector<string> dicts(stickers);
        for(auto& d: dicts){
            d=translate(d);
        }
        
        return helper(dicts,translate(target));
    }
    
    string translate(string s){
        string res(26,0);
        for(auto &c: s){
            res[c-'a']++;
        }
        return res;
    }
    
    int helper(vector<string>& dicts, string goal){
        if(goal=="#") return -1;
        if(dp.count(goal)) return dp[goal];
        int res=INT_MAX;
        
        for(auto& d: dicts){
            int tmp=helper(dicts,cut(goal,d));
            if(tmp==-1) continue;
            res=min(res,tmp);
        }
        if(res==INT_MAX) return -1;
        res+=1;//current step
        
        dp[goal]=res;
        return res;
    }
    
    string cut(string goal, string d){
        string res=goal;
        for(int i=0;i<26;i++){
            res[i]-=d[i];
            if(res[i]<0) res[i]=0;
        }
        if(res==goal) return "#";//can't cut anything
        return res;
    }
};
```