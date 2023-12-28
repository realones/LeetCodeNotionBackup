# Find the Shortest Superstring

#: 943
Difficult: Hard
Tags: Graph, Permutation, TSP/HamiltonianPath, complete_graph, dp-bitMask, search
link: https://leetcode.com/problems/find-the-shortest-superstring/
Priority: High
Created time: March 14, 2023 9:52 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, wp动归套路
related: Number of Squareful Arrays (Number%20of%20Squareful%20Arrays%200de26cd61dca45819145a897da4cda1b.md)

Given an array of strings `words`, return *the smallest string that contains each string in* `words` *as a substring*. If there are multiple valid strings of the smallest length, return **any of them**.

You may assume that no string in `words` is a substring of another string in `words`.

**Example 1:**

```
Input: words = ["alex","loves","leetcode"]
Output: "alexlovesleetcode"
Explanation: All permutations of "alex","loves","leetcode" would also be accepted.

```

**Example 2:**

```
Input: words = ["catg","ctaagt","gcta","ttca","atgcatc"]
Output: "gctaagttcatgcatc"

```

**Constraints:**

- `1 <= words.length <= 12`
- `1 <= words[i].length <= 20`
- `words[i]` consists of lowercase English letters.
- All the strings of `words` are **unique**.

comment: MST? no, since each node only visit once.

Solution:

```cpp
/*
A,B,C

A = abc
B = bcd
C = cde

possible res: find the shortest one
ABC => abcde
ACB
BAC
BCA
CAB
CBA

Travelling salesman problem: (NP)
A->B = 1 : the distance of from A to superStr AB is 1
B->C = 1
A->C = 2
B->A = 3
C->A = 3
C->B = 3

find the solution each node visit only once, and the total path distance is the shortest

DP + BitMask
*/

/*
pseudocode:
dp[set][bit]: 
    the shortest path if we have traveled the cities in set (bit val = 1)
    and end in `bit` city

A,B,C,D

dp[BCD][B] min= 
    dp[CD][C] + dist(C,B)
    dp[CD][D] + dist(D,B)

for set = 0001 : 1111 
    for bit = ..
        dp[set][bit] min=@ 
        set_prev = set - bit
        for bit_prev = ..
            @dp[set_prev][bit_prev] + dist(bit_pre, bit)
            
            
复杂度分析：
时间复杂度：O(n^2 * 2^n)，其中 n 是字符串数组 A 的长度。
空间复杂度：O(n * 2^n)。

*/

class Solution {
public:
    string shortestSuperstring(vector<string>& words) {
        int N=words.size();        
        //get dist
        vector<vector<int>> dist(N,vector<int>(N,-1));
        for(int i=0;i<N;i++){
            for(int j=0;j<N;j++){
                if(i!=j) dist[i][j]=getDist(words[i],words[j]);
            }
        }

        int M=(1<<N);
				//dp mast(visited nodes) bit(last visited)
        vector<vector<int>> dp(M,vector<int>(N,INT_MAX/2));
        vector<vector<int>> parent(M,vector<int>(N,-1));//to org
        
        //init dp //to org
        for(int i=0; i<N; i++){
            dp[1<<i][i] = words[i].size();
        }
        
        //dp process
        for(int mask=0; mask<M; mask++){
            for(int bit=0; bit<N; bit++){
                if((mask&(1<<bit)) == 0) continue; //bit not in mask //to org
                int mask_prev = mask - (1<<bit);
                if(mask_prev == 0) continue; //safe guard
                for(int bit_prev=0; bit_prev<N; bit_prev++){
                    if((mask_prev&(1<<bit_prev)) == 0) continue; //bit_prev not in mask_prev
                    if(dp[mask][bit] > dp[mask_prev][bit_prev] + dist[bit_prev][bit]){
                        dp[mask][bit]=dp[mask_prev][bit_prev] + dist[bit_prev][bit];
                        parent[mask][bit] = bit_prev;//to org
                    }
                }
            }
        }
        
        int totalDist = INT_MAX;
        int bit_last=-1;
        for(int bit=0;bit<N;bit++){
            if(totalDist>dp.back()[bit]){
                totalDist=dp.back()[bit];
                bit_last=bit;
            }
        }
        
        //back track to build res
        int mask = M-1;
        int bit=bit_last;
        vector<int>path({bit});
        while (parent[mask][bit]!=-1)
        {
            int next = parent[mask][bit];
            path.push_back(next);
            mask = mask^(1<<bit);
            bit = next;
        }
        
        reverse(path.begin(), path.end());
        
        string resStr=words[path[0]];
        for(int i=1;i<path.size();i++){
            resStr = combine(resStr,words[path[i]]);
        }
        return resStr;
    }
    
    int getDist(string S, string T)
    {
        for (int k=min(S.size(), T.size()); k>=0; k--)
        {
            if (S.substr(S.size()-k)==T.substr(0,k))
                return T.size()-k;
        }
        return T.size();
    }
    
    string combine(string S, string T)
    {
        for (int k=min(S.size(), T.size()); k>=0; k--)
        {
            if (S.substr(S.size()-k)==T.substr(0,k))
                return S+T.substr(k);
        }
        return S+T;
    }
};
```