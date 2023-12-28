# Jump Game IV

#: 1345
Difficult: Hard
Tags: Array, BFS, Hash
link: https://leetcode.com/problems/jump-game-iv/description/
Priority: Medium
Status: CalcuComplexity
Created time: December 5, 2023 8:23 PM
Last edited time: December 5, 2023 8:24 PM
source: ticktokFreq

Given an array of integers `arr`, you are initially positioned at the first index of the array.

In one step you can jump from index `i` to index:

- `i + 1` where: `i + 1 < arr.length`.
- `i - 1` where: `i - 1 >= 0`.
- `j` where: `arr[i] == arr[j]` and `i != j`.

Return *the minimum number of steps* to reach the **last index** of the array.

Notice that you can not jump outside of the array at any time.

**Example 1:**

```
Input: arr = [100,-23,-23,404,100,23,23,23,3,404]
Output: 3
Explanation: You need three jumps from index 0 --> 4 --> 3 --> 9. Note that index 9 is the last index of the array.

```

**Example 2:**

```
Input: arr = [7]
Output: 0
Explanation: Start index is the last index. You do not need to jump.

```

**Example 3:**

```
Input: arr = [7,6,9,6,9,6,9,7]
Output: 1
Explanation: You can jump directly from index 0 to index 7 which is last index of the array.

```

**Constraints:**

- `1 <= arr.length <= 5 * 104`
- `108 <= arr[i] <= 108`

comment:

remove edges for future nodes to make it O(V)

Solution BFS:

```cpp
class Solution {
public:
    int minJumps(vector<int>& nums) {
        //build mp[val->idxs] used as graph also
        //T: O(n)
        //S: O(n)
        int n=nums.size();// if(n==0) {return 0;}
        unordered_map<int, unordered_set<int>> mp;//val - idxs
        for(int i=0;i<n;i++){
            mp[nums[i]].insert(i);
        }
        // find nearest path from idx 0 to idx last
        // T: O(V) which is O(n)
        // S: O(1)
        queue<int> q;
        vector<int> visited(n,0);
        
        q.push(0);
        visited[0]=true;

        int step=0;
        while(!q.empty()){
            int sz=q.size();
            for(int i=0; i<sz; i++){
                int cur=q.front(); q.pop();
                if(cur==n-1) return step;

                unordered_set<int>& nxts = mp[nums[cur]];
                //ok to insert, since marked as visited
                if(cur-1>=0) nxts.insert(cur-1);
                if(cur+1<n) nxts.insert(cur+1);
                for(auto nxt: nxts){
                    if(nxt==cur) continue;
                    if(visited[nxt]) continue;
                    visited[nxt]=true;
                    q.push(nxt);
                }
                nxts.clear();//from O(V+E) to O(E)
            }
            step++;
        }
        return -1;
    }
};
/*
jump from i to 
    i+1: i+1<n
    i-1: i-1>=0
    j:   nums[i]==nums[j], i!=j
return:
    min steps to reach last idx
================================
nums:
    len: 1~5e4
    val: -1e8 ~ 1e8
================================
thoughts:
    jump from start, and jump from end - same logic
    dp[i]: min steps from i to last idx
    dp[i] = min(dp[i-1], dp[i+1], dp[j])
        i->j:
            maintain mp[val]->idxs
            can't jump back - visited set
            shortest path - bfs
    dp[last]=0;
    return :dp[0]
================================
bfs:
    build graph
        mp[i]=st of [j], remove it self
    find nearest path from idx 0 to idx last
*/
```