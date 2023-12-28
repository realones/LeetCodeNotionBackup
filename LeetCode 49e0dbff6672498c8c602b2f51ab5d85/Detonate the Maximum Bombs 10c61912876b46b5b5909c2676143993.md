# Detonate the Maximum Bombs

#: 2101
Difficult: Medium
Tags: Array, BFS, Graph
link: https://leetcode.com/problems/detonate-the-maximum-bombs/
Priority: Medium
Created time: June 2, 2023 3:16 PM
Last edited time: October 12, 2023 2:56 PM
source: leetcode_daily

You are given a list of bombs. The **range** of a bomb is defined as the area where its effect can be felt. This area is in the shape of a **circle** with the center as the location of the bomb.

The bombs are represented by a **0-indexed** 2D integer array `bombs` where `bombs[i] = [xi, yi, ri]`. `xi` and `yi` denote the X-coordinate and Y-coordinate of the location of the `ith` bomb, whereas `ri` denotes the **radius** of its range.

You may choose to detonate a **single** bomb. When a bomb is detonated, it will detonate **all bombs** that lie in its range. These bombs will further detonate the bombs that lie in their ranges.

Given the list of `bombs`, return *the **maximum** number of bombs that can be detonated if you are allowed to detonate **only one** bomb*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-3.png](https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-3.png)

```
Input: bombs = [[2,1,3],[6,1,4]]
Output: 2
Explanation:
The above figure shows the positions and ranges of the 2 bombs.
If we detonate the left bomb, the right bomb will not be affected.
But if we detonate the right bomb, both bombs will be detonated.
So the maximum bombs that can be detonated is max(1, 2) = 2.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-2.png](https://assets.leetcode.com/uploads/2021/11/06/desmos-eg-2.png)

```
Input: bombs = [[1,1,5],[10,10,5]]
Output: 1
Explanation:
Detonating either bomb will not detonate the other bomb, so the maximum number of bombs that can be detonated is 1.

```

**Example 3:**

![https://assets.leetcode.com/uploads/2021/11/07/desmos-eg1.png](https://assets.leetcode.com/uploads/2021/11/07/desmos-eg1.png)

```
Input: bombs = [[1,2,3],[2,3,1],[3,4,2],[4,5,3],[5,6,4]]
Output: 5
Explanation:
The best bomb to detonate is bomb 0 because:
- Bomb 0 detonates bombs 1 and 2. The red circle denotes the range of bomb 0.
- Bomb 2 detonates bomb 3. The blue circle denotes the range of bomb 2.
- Bomb 3 detonates bomb 4. The green circle denotes the range of bomb 3.
Thus all 5 bombs are detonated.

```

**Constraints:**

- `1 <= bombs.length <= 100`
- `bombs[i].length == 3`
- `1 <= xi, yi, ri <= 105`

```cpp
class Solution {
public:
    int maximumDetonation(vector<vector<int>>& bombs) {
        //build directed graph
        int n=bombs.size();
        vector<unordered_set<int>> g(n);
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(i==j) continue;
                if(isConnected(bombs[i],bombs[j])){
                    g[i].insert(j);
                }
            }
        }
        
        //for each node, bfs
        int res=1;
        for(int i=0; i<n; i++){
            int sz=0;
            vector<bool> visited(n,0);
            queue<int> q;
            visited[i]=true;
            q.push(i);
            while(!q.empty()){
                int cur=q.front(); q.pop();
                sz++;
                for(auto nxt: g[cur]){
                    if(visited[nxt]) continue;
                    visited[nxt]=true;
                    q.push(nxt);
                }
            }
            res=max(res,sz);
        }
        return res;
    }
    
    bool isConnected(vector<int>& s, vector<int>&t){
        int x1=s[0],y1=s[1],r=s[2];
        int x2=t[0],y2=t[1];
        return pow((x1-x2),2)+pow((y1-y2),2) <= pow(r,2);
    }
};
```