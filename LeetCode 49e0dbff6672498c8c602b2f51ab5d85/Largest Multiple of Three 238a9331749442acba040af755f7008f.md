# Largest Multiple of Three

#: 1363
Difficult: Hard
Tags: Array, Greedy, dp-TimeSeqFromLastOne-Power, parent, stateMachine
link: https://leetcode.com/problems/largest-multiple-of-three/
Priority: High
Status: HardToImplement, No Idea At First, toRevisit, toSummarize
Created time: June 16, 2023 1:45 AM
Last edited time: October 17, 2023 6:24 PM
source: leetcode_daily

Given an array of digits `digits`, return *the largest multiple of **three** that can be formed by concatenating some of the given digits in **any order***. If there is no answer return an empty string.

Since the answer may not fit in an integer data type, return the answer as a string. Note that the returning answer must not contain unnecessary leading zeros.

**Example 1:**

```
Input: digits = [8,1,9]
Output: "981"

```

**Example 2:**

```
Input: digits = [8,6,7,1,0]
Output: "8760"

```

**Example 3:**

```
Input: digits = [1]
Output: ""

```

**Constraints:**

- `1 <= digits.length <= 104`
- `0 <= digits[i] <= 9`

Solution 1: Math

```cpp
class Solution {
public:
    string largestMultipleOfThree(vector<int>& digits) {
        sort(digits.begin(), digits.end(),greater<>());

        int mod=0;
        for(auto d: digits){
            if(d%3==0) continue;
            else if(d%3==1) mod=(mod+1)%3;
            else mod=(mod+2)%3;
        }

        string res="";
        for(auto d: digits){
            res+=to_string(d);
        }

        if(mod==0) return res[0]=='0'?"0":res;
        else if(mod==1){
            for(int i=res.size()-1;i>=0;i--){
                if((res[i]-'0')%3 == 1) {
                    res.erase(i,1);
                    return res[0]=='0'?"0":res;
                }
            }
            int cnt=2;
            for(int i=res.size()-1;i>=0;i--){
                if((res[i]-'0')%3 == 2) {
                    res.erase(i,1);
                    cnt--;
                    if(cnt==0) return res[0]=='0'?"0":res;
                }
            }
        }
        else{
            for(int i=res.size()-1;i>=0;i--){
                if((res[i]-'0')%3 == 2) {
                    res.erase(i,1);
                    return res[0]=='0'?"0":res;
                }
            }
            int cnt=2;
            for(int i=res.size()-1;i>=0;i--){
                if((res[i]-'0')%3 == 1) {
                    res.erase(i,1);
                    cnt--;
                    if(cnt==0) return res[0]=='0'?"0":res;
                }
            }
        }
        return "-1";
    }
};

/*
if totalSum%3==0, select all digits
if totalSum%3==1, 
    exclude min one element which %=1, 
    exclude min two element which %=2, if above not possible
if totalSum%3==2,
    exclude min one element which %=2, 
    exclude min two element which %=1, if above not possible
*/
```

Solution 3: DP - TLE - same with below but easy to read, as as result, TLE

```cpp
class Solution {
public:
    string largestMultipleOfThree(vector<int>& digits) {
        int n=digits.size();
        sort(digits.begin(), digits.end(), greater<>());
        digits.insert(digits.begin(),0);
        //dp
        vector<vector<string>> dp(n+1,vector<string>(3,"#"));//init as invalid
        //init
        dp[0][0] = "";//init
        dp[0][1] = "#";//invalid
        dp[0][2] = "#";//invalid
        //process
        for(int i=1;i<n+1;i++){
            int d=digits[i];
            for(int j=0; j<3; j++){
                string notPick=dp[i-1][j];
                string pick=dp[i-1][(j-d%3+3)%3]+to_string(d);
                dp[i][j]=maxLen(notPick,pick);
            }
        }

        string res=dp.back()[0];
        if(res[0]=='0') return "0";
        return res;
    }
    
    string maxLen(string& s1, string& s2){
        if(!s1.empty() && s1[0]=='#' && !s2.empty() && s2[0]=='#') return "#";
        else if(!s1.empty() && s1[0]=='#') return s2;
        else if(!s2.empty() && s2[0]=='#') return s1;
        else if(s2.size()==s1.size()) return s1;
        else return s1.size()>s2.size()? s1:s2;
    }
};

```

Solution 2: DP

```cpp
class Solution {    
public:
    string largestMultipleOfThree(vector<int>& digits) 
    {
        sort(digits.begin(), digits.end());
        reverse(digits.begin(), digits.end());

        int n = digits.size();
        digits.insert(digits.begin(), 0);
        
        string dp[n+1][3];
        dp[0][0] = "", dp[0][1] = "#", dp[0][2] = "#";
        
        for (int i=1; i<=n; i++)        
            for (int j=0; j<3; j++)
            {                
                if (dp[i-1][j]=="#" && dp[i-1][(j-digits[i]%3+3)%3]=="#")
                {
                    dp[i][j] = "#";
                }
                else if (dp[i-1][j]=="#")
                {
                    dp[i][j] = dp[i-1][(j-digits[i]%3+3)%3] + to_string(digits[i]);
                }
                else if (dp[i-1][(j-digits[i]%3+3)%3]=="#")
                {
                    dp[i][j] = dp[i-1][j];
                }
                else if (dp[i-1][j].size() >= dp[i-1][(j-digits[i]%3+3)%3].size()+1)
                {
                    dp[i][j] = dp[i-1][j];
                }
                else
                {
                    dp[i][j] = dp[i-1][(j-digits[i]%3+3)%3] + to_string(digits[i]);
                }
            }

        string ret = dp[n][0];
        if (ret[0]=='0')
            return "0";
        else
            return ret;
    }
};
/*
DP - state machine - timeSeqLastOne
dp[i][j]: the largest str (%3=j) we can construct using digits[1:i]

X X X X X i X X X 
dp[i][0]
dp[i][1]
dp[i][2]

dp[i][j] = better of
1. do not pick digits[i]
    dp[i][j] = dp[i-1][j]
2. pick digits[i]:
    dp[i][j] = dp[i-1][j-curMod] + digit[i]

init: 
    dp[0][0]=""
    dp[0][1]="#" invalid
    dp[0][2]="#" invalid
    dp[x][y]="#" invalid

return: dp.back()[0];

better of (dp[i-1][j], dp[i-1][j-k] + digit[i-1])
    take the longer one
    if same size:
        pre sort digits large to small
        use s1, since digit[i] is smallest digit
*/
```

Solution 4: Improve from solution 2, use parent to trace back

```cpp
https://www.youtube.com/watch?v=yjyv2_4_Abs
```