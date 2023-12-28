# Minimum Cost to Make All Characters Equal

#: 2712
Difficult: Medium
Tags: Greedy, dp-TimeSeqFromLastOne-Power, stateMachine
link: https://leetcode.com/problems/minimum-cost-to-make-all-characters-equal/
Priority: High
Status: Dig More, More Solution
Created time: May 27, 2023 10:38 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** binary string `s` of length `n` on which you can apply two types of operations:

- Choose an index `i` and invert all characters from index `0` to index `i` (both inclusive), with a cost of `i + 1`
- Choose an index `i` and invert all characters from index `i` to index `n - 1` (both inclusive), with a cost of `n - i`

Return *the **minimum cost** to make all characters of the string **equal***.

**Invert** a character means if its value is '0' it becomes '1' and vice-versa.

**Example 1:**

```
Input: s = "0011"
Output: 2
Explanation: Apply the second operation withi = 2 to obtains = "0000" for a cost of 2. It can be shown that 2 is the minimum cost to make all characters equal.

```

**Example 2:**

```
Input: s = "010101"
Output: 9
Explanation: Apply the first operation with i = 2 to obtain s = "101101" for a cost of 3.
Apply the first operation with i = 1 to obtain s = "011101" for a cost of 2.
Apply the first operation with i = 0 to obtain s = "111101" for a cost of 1.
Apply the second operation with i = 4 to obtain s = "111110" for a cost of 2.
Apply the second operation with i = 5 to obtain s = "111111" for a cost of 1.
The total cost to make all characters equal is 9. It can be shown that 9 is the minimum cost to make all characters equal.

```

**Constraints:**

- `1 <= s.length == n <= 105`
- `s[i]` is either `'0'` or `'1'`

Solution: improved from following solution - dp

```cpp
typedef long long ll;
class Solution {
public:
    long long minimumCost(string s) {
        int n=s.size();
        //l2r
        vector<vector<ll>> l2r(2,vector<ll>(n,0));
        l2r[0][0]=(s[0]=='0')?0:1;
        l2r[1][0]=(s[0]=='1')?0:1;
        for(int i=1; i<n; i++){
            l2r[0][i]=
                s[i]=='0'?
                    l2r[0][i-1]:
                    l2r[1][i-1] + i+1;
            l2r[1][i]=
                s[i]=='1'?
                    l2r[1][i-1]:
                    l2r[0][i-1] + i+1;
        }
        //r2l
        vector<vector<ll>> r2l(2,vector<ll>(n,0));
        r2l[0][n-1]=(s[n-1]=='0')?0:1;
        r2l[1][n-1]=(s[n-1]=='1')?0:1;
        for(int i=n-2; i>=0; i--){
            r2l[0][i]=
                s[i]=='0'?
                    r2l[0][i+1]:
                    r2l[1][i+1] + n-i;
            r2l[1][i]=
                s[i]=='1'?
                    r2l[1][i+1]:
                    r2l[0][i+1] + n-i;
        }
        //calcu res
        ll res=LLONG_MAX;
        for(int i=0; i<=n; i++){
           res=min({res,
                   (i-1>=0?l2r[0][i-1]:0) + (i<=n-1?r2l[0][i]:0),
                   (i-1>=0?l2r[1][i-1]:0) + (i<=n-1?r2l[1][i]:0)
                   });
        }
        return res;
    }
};

/*
dp
l2r [0][0..i]: cost to convert s[0..i] to all 0s
    
i: 0...n-1
l2r [0][0..i] = 
    if s[i]==0 dp[0][i-1]
    if s[i]==1 dp[1][i-1] + i+1
        
l2r [1][0..i] = 
    if s[i]==1 dp[1][i-1]
    if s[i]==0 dp[0][i-1] + i+1

i: n-1..0        
r2l [0][i..n-1] = 
    if s[i]==0 dp[0][i+1]
    if s[i]==1 dp[1][i+1] + n-i
        
r2l [1][0..i] = 
    if s[i]==1 dp[1][i+1]
    if s[i]==0 dp[0][i+1] + n-i
        
*/
```

Solution: Memory Limit Exceeded

```cpp
class Solution {
public:
    unordered_map<string,long long> dpL0;
    unordered_map<string,long long> dpR0;
    unordered_map<string,long long> dpL1;
    unordered_map<string,long long> dpR1;
    
    long long minimumCost(string s) {
        int n=s.size();
        long long res=INT_MAX;
        
        
        //000000
        for(int sz=0; sz<=n; sz++){
            string l=s.substr(0,sz);
            string r=s.substr(sz);
            // std::cout << "ls" << " -- " << l << std::endl;
            // std::cout << "rs" << " -- " << r << std::endl;
            long long a=revertL0(l);
            // std::cout << "a" << " -- " << a << std::endl;
            long long b=revertR0(r);
            // std::cout << "b" << " -- " << b << std::endl;
            
            long long tmp=a+b;
            // std::cout << "tmp" << " -- " << tmp << std::endl;
            res=min(res,tmp);      
            
        }
        // std::cout << "res" << " -- " << res << std::endl;
        // std::cout << "111" << std::endl;
        //111111
        for(int sz=0; sz<=n; sz++){
            string l=s.substr(0,sz);
            string r=s.substr(sz);
            // std::cout << "ls" << " -- " << l << std::endl;
            long long a=revertL1(l);
            // std::cout << "a" << " -- " << a << std::endl;
            // std::cout << "rs" << " -- " << r << std::endl;
            long long b=revertR1(r);
            // std::cout << "b" << " -- " << b << std::endl;
            
            
            long long tmp=a+b;
            // std::cout << "tmp" << " -- " << tmp << std::endl;
            res=min(res,tmp);
            
        }
        return res;
        
    }
    
    void revert(string& s){
        for(auto& c: s){
            if(c=='0') c='1';
            else{ c='0';}
        }
    }
    
    long long revertL0(string s){
        if(s.empty()) return 0;
        if(dpL0.count(s)) return dpL0[s];
        int n=s.size();
        int i=n-1;
        // std::cout << "s" << " -- " << s << std::endl;
        // std::cout << "i" << " -- " << i << std::endl;
        while(i>=0 && s[i]=='0') i--;
        // std::cout << "i" << " -- " << i << std::endl;
        string l=s.substr(0,i+1);
        string r=s.substr(i+1);
        // std::cout << "ll" << " -- " << l << std::endl;
        // std::cout << "rr" << " -- " << r << std::endl;
        revert(l);
        dpL0[s] = revertL0(l)+l.size();
        return dpL0[s];
    }
    
    long long revertR0(string s){
        if(s.empty()) return 0;
        if(dpR0.count(s)) return dpR0[s];
        int n=s.size();
        int i=0;
        while(i<=n-1 && s[i]=='0') i++;
        string l=s.substr(0,i);
        string r=s.substr(i);
        // std::cout << "l" << " -- " << l << std::endl;
        // std::cout << "r" << " -- " << r << std::endl;
        revert(r);
        dpR0[s] = revertR0(r)+r.size();
        return dpR0[s];
    }
    
    long long revertL1(string s){
        if(s.empty()) return 0;
        if(dpL1.count(s)) return dpL1[s];
        int n=s.size();
        int i=n-1;
        while(i>=0 && s[i]=='1') i--;
        string l=s.substr(0,i+1);
        // std::cout << "l" << " -- " << l << std::endl;
        string r=s.substr(i+1);
        // std::cout << "r" << " -- " << r << std::endl;
        revert(l);
        dpL1[s] = revertL1(l)+l.size();
        return dpL1[s];
    }
    
    long long revertR1(string s){
        if(s.empty()) return 0;
        if(dpR1.count(s)) return dpR1[s];
        int n=s.size();
        int i=0;
        while(i<=n-1 && s[i]=='1') i++;
        string l=s.substr(0,i);
        string r=s.substr(i);
        revert(r);
        dpR1[s] = revertR1(r)+r.size();
        return dpR1[s];
    }
};

/*
to 00000
dp 0~n-1
dp 
    
to 11111
*/
```