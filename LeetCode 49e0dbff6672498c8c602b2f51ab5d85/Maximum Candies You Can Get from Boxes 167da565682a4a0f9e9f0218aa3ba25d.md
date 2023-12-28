# Maximum Candies You Can Get from Boxes

#: 1298
Difficult: Hard
Tags: Array, BFS, Graph, simulation
link: https://leetcode.com/problems/maximum-candies-you-can-get-from-boxes/
Priority: High
Status: Dig More, HardToImplement, checkBetterSolution, toRevisit
Created time: June 28, 2023 12:08 AM
Last edited time: October 17, 2023 6:24 PM
source: HuaHua

You have `n` boxes labeled from `0` to `n - 1`. You are given four arrays: `status`, `candies`, `keys`, and `containedBoxes` where:

- `status[i]` is `1` if the `ith` box is open and `0` if the `ith` box is closed,
- `candies[i]` is the number of candies in the `ith` box,
- `keys[i]` is a list of the labels of the boxes you can open after opening the `ith` box.
- `containedBoxes[i]` is a list of the boxes you found inside the `ith` box.

You are given an integer array `initialBoxes` that contains the labels of the boxes you initially have. You can take all the candies in **any open box** and you can use the keys in it to open new boxes and you also can use the boxes you find in it.

Return *the maximum number of candies you can get following the rules above*.

**Example 1:**

```
Input: status = [1,0,1,0], candies = [7,5,4,100], keys = [[],[],[1],[]], containedBoxes = [[1,2],[3],[],[]], initialBoxes = [0]
Output: 16
Explanation: You will be initially given box 0. You will find 7 candies in it and boxes 1 and 2.
Box 1 is closed and you do not have a key for it so you will open box 2. You will find 4 candies and a key to box 1 in box 2.
In box 1, you will find 5 candies and box 3 but you will not find a key to box 3 so box 3 will remain closed.
Total number of candies collected = 7 + 4 + 5 = 16 candy.

```

**Example 2:**

```
Input: status = [1,0,0,0,0,0], candies = [1,1,1,1,1,1], keys = [[1,2,3,4,5],[],[],[],[],[]], containedBoxes = [[1,2,3,4,5],[],[],[],[],[]], initialBoxes = [0]
Output: 6
Explanation: You have initially box 0. Opening it you can find boxes 1,2,3,4 and 5 and their keys.
The total number of candies will be 6.

```

**Constraints:**

- `n == status.length == candies.length == keys.length == containedBoxes.length`
- `1 <= n <= 1000`
- `status[i]` is either `0` or `1`.
- `1 <= candies[i] <= 1000`
- `0 <= keys[i].length <= n`
- `0 <= keys[i][j] < n`
- All values of `keys[i]` are **unique**.
- `0 <= containedBoxes[i].length <= n`
- `0 <= containedBoxes[i][j] < n`
- All values of `containedBoxes[i]` are unique.
- Each box is contained in one box at most.
- `0 <= initialBoxes.length <= n`
- `0 <= initialBoxes[i] < n`

todo: need to repractice this issue, be aware why topSort not working

Solution BFS - simulation:

```cpp
class Solution {
public:
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) {
        int n=status.size();
        unordered_set<int> myBoxes;
        unordered_set<int> myKeys;
        for(auto boxId: initialBoxes) myBoxes.insert(boxId);

        queue<int> q;
        vector<int> visited(n,0);
        for(int b_id: initialBoxes){
            if(status[b_id]) 
            {   
                q.push(b_id);
                visited[b_id]=1;
            }
        }
        int res=0;
        while(!q.empty()){
            int cur=q.front(); q.pop();
            res+=candies[cur];
            for(auto k: keys[cur]){
                myKeys.insert(k);
                if(visited[k]) continue;
                if(!myBoxes.count(k)) continue;
                visited[k]=1;
                q.push(k);
            }
            for(auto b: containedBoxes[cur]){
                myBoxes.insert(b);
                if(visited[b]) continue;
                if(status[b] || myKeys.count(b)) {
                    q.push(b);
                    visited[b]=1;
                }
            }
        }
        return res;
    }
};
/*
status: is it possible to be locked by multi locks? assuming No
keys: so if 1->3 and 2->3, if we only have 1 but not two, can we open 3? assuming Yes
is it possible to be one box contained by two boxes? assuming No

initialBoxes - what boxs you have -> so if you have a key but not the box, you still can't open it? then not TopSort.

================================
a set of boxes you have
a set of keys you have (muti same key exist or not?)

init boxes into box st -> open one into q for bfs
for new boxes -> into st, 
for new keys -> into st
*/
```

Solution BFS - lower performance than above:

comment: 可以加括号的地方，一定要加括号！！！！！！免得出错！！！！！这里写错了debug了半天

```cpp
				for(int b_id: initialBoxes){
            if(status[b_id]) q.push(b_id);
            visited[b_id]=1;
        }
```

```cpp
class Solution {
public:
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) {
        int n=status.size();
        unordered_set<int> myBoxes;
        unordered_set<int> myKeys;
        for(auto boxId: initialBoxes) myBoxes.insert(boxId);

        queue<int> q;
        vector<int> visited(n,0);
        for(int b_id: initialBoxes){
            if(status[b_id]) {
                q.push(b_id);
                visited[b_id]=1;
            }
        }
        int res=0;
        while(!q.empty()){
            int cur=q.front(); q.pop();
            res+=candies[cur];
            myBoxes.erase(cur);
            myKeys.insert(keys[cur].begin(), keys[cur].end());
            myBoxes.insert(containedBoxes[cur].begin(),containedBoxes[cur].end());
            for(int nei: myBoxes){
                if(visited[nei]) continue;
                if(!status[nei] && !myKeys.count(nei)) continue;
                visited[nei]=1;
                q.push(nei);
            }
        }
        return res;
    }
};
/*
status: is it possible to be locked by multi locks? assuming No
keys: so if 1->3 and 2->3, if we only have 1 but not two, can we open 3? assuming Yes
is it possible to be one box contained by two boxes? assuming No

initialBoxes - what boxs you have -> so if you have a key but not the box, you still can't open it? then not TopSort.

================================
a set of boxes you have
a set of keys you have (muti same key exist or not?)

init boxes into box st -> open one into q for bfs
for new boxes -> into st, 
for new keys -> into st
*/
```

Solution TopSort- Wrong

```cpp
class Solution {
public:
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) {
        int n=status.size();
        
        unordered_map<int,unordered_set<int>> g;
        vector<int> ind(n,0);
        
        //key lock dependencies
        for(int i=0; i<n; i++){
            for(int x: keys[i]){
                g[i].insert(x);
                ind[x]++;
            }
        }
        
        //g[-1] to all closed boxes
        for(int i=0; i<n; i++){
            if(status[i]==0) {
                g[-1].insert(i);
                ind[i]++;
            }
        }

        //containing dependencies
        for(int i=0; i<n; i++){
            for(auto x: containedBoxes[i]){
                if(!g[i].count(x)){
                    g[i].insert(x);
                    ind[x]++;
                }
            }
        }

        //bfs with inital boxes
        queue<int> q;
        int res=0;
        for(auto src: initialBoxes){
            if(ind[src]==0) q.push(src);
        }
        while(!q.empty()){
            int cur=q.front(); q.pop();
            res+=candies[cur];
            for(auto nei: g[cur]){
                ind[nei]--;
                if(ind[nei]==0) q.push(nei);
            }
        }
        return res;
    }
};
```