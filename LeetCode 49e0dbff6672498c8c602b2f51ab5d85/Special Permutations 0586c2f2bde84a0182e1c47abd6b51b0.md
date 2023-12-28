# Special Permutations

#: 2741
Difficult: Medium
link: https://leetcode.com/problems/special-permutations/
Priority: High
Status: In Progress, No Idea At First
Created time: June 17, 2023 9:52 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Contests

You are given a **0-indexed** integer array `nums` containing `n` **distinct** positive integers. A permutation of `nums` is called special if:

- For all indexes `0 <= i < n - 1`, either `nums[i] % nums[i+1] == 0` or `nums[i+1] % nums[i] == 0`.

Return *the total number of special permutations.* As the answer could be large, return it **modulo** `109 + 7`.

**Example 1:**

```
Input: nums = [2,3,6]
Output: 2
Explanation: [3,6,2] and [2,6,3] are the two special permutations of nums.

```

**Example 2:**

```
Input: nums = [1,4,3]
Output: 2
Explanation: [3,1,4] and [4,1,3] are the two special permutations of nums.

```

**Constraints:**

- `2 <= nums.length <= 14`
- `1 <= nums[i] <= 109`

Solution TLE:

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int res=0;
    int specialPerm(vector<int>& nums) {
        int n=nums.size();
        vector<int> visited(n,0);
        vector<int> tmp;
        backtracking(nums,visited,tmp);
        return res;
    }
    
    void backtracking(vector<int>& nums, vector<int>& visited, vector<int>& tmp){
        int n=nums.size();
        if(tmp.size()==n) {res++;return;}
        for(int i=0; i<n; i++){
            if(visited[i]) continue;
            if(tmp.size()==0 || nums[i]%tmp.back()==0 || tmp.back()%nums[i]==0){
                tmp.push_back(nums[i]); visited[i]=1;
                backtracking(nums,visited,tmp);
                tmp.pop_back(); visited[i]=0;
            }
        }
    }
};
```

Solution TLE:

```cpp
using ll=long long;
ll M=1e9+7;

class Solution {
public:
    int specialPerm(vector<int>& nums) {
        int n=nums.size();
        
        vector<unordered_set<int>> g(n);
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(i==j) continue;
                if(nums[i]%nums[j]==0 || nums[j]%nums[i]==0) {
                    g[i].insert(j);
                }
            }
        }
        
        int res=0;
        vector<bool> visited(n,0);
        int visitedCnt=0;
        for(int i=0; i<n; i++){
            res+=dfs(g, visited, visitedCnt, i);
        }
        return res;
    }
    
    int dfs(vector<unordered_set<int>>& g, vector<bool>& visited, int& visitedCnt, int node){
        if(visitedCnt==g.size()-1) return 1;
        int res=0;
        visited[node]=1;visitedCnt++;
        for(auto nxt: g[node]){
            if(!visited[nxt]){
                res+=dfs(g,visited,visitedCnt,nxt);
            }
        }
        visited[node]=0;visitedCnt--;
        return res;
    }
};
```