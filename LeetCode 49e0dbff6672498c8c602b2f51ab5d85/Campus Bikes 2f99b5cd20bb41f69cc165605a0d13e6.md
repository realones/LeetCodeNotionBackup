# Campus Bikes

#: 1057
Difficult: Medium
Tags: Array, Greedy, Hash, OrderSet, Sort, simulation
link: https://leetcode.com/problems/campus-bikes/description/
Priority: High
Status: HardToImplement, More Solution, checkBetterSolution
Created time: November 12, 2023 4:38 PM
Last edited time: November 12, 2023 4:40 PM
source: leetcode_daily

On a campus represented on the X-Y plane, there are `n` workers and `m` bikes, with `n <= m`.

You are given an array `workers` of length `n` where `workers[i] = [xi, yi]` is the position of the `ith` worker. You are also given an array `bikes` of length `m` where `bikes[j] = [xj, yj]` is the position of the `jth` bike. All the given positions are **unique**.

Assign a bike to each worker. Among the available bikes and workers, we choose the `(workeri, bikej)` pair with the shortest **Manhattan distance** between each other and assign the bike to that worker.

If there are multiple `(workeri, bikej)` pairs with the same shortest **Manhattan distance**, we choose the pair with **the smallest worker index**. If there are multiple ways to do that, we choose the pair with **the smallest bike index**. Repeat this process until there are no available workers.

Return *an array* `answer` *of length* `n`*, where* `answer[i]` *is the index (**0-indexed**) of the bike that the* `ith` *worker is assigned to*.

The **Manhattan distance** between two points `p1` and `p2` is `Manhattan(p1, p2) = |p1.x - p2.x| + |p1.y - p2.y|`.

**Example 1:**

![https://assets.leetcode.com/uploads/2019/03/06/1261_example_1_v2.png](https://assets.leetcode.com/uploads/2019/03/06/1261_example_1_v2.png)

```
Input: workers = [[0,0],[2,1]], bikes = [[1,2],[3,3]]
Output: [1,0]
Explanation: Worker 1 grabs Bike 0 as they are closest (without ties), and Worker 0 is assigned Bike 1. So the output is [1, 0].

```

**Example 2:**

![https://assets.leetcode.com/uploads/2019/03/06/1261_example_2_v2.png](https://assets.leetcode.com/uploads/2019/03/06/1261_example_2_v2.png)

```
Input: workers = [[0,0],[1,1],[2,0]], bikes = [[1,0],[2,2],[2,1]]
Output: [0,2,1]
Explanation: Worker 0 grabs Bike 0 at first. Worker 1 and Worker 2 share the same distance to Bike 2, thus Worker 1 is assigned to Bike 2, and Worker 2 will take Bike 1. So the output is [0,2,1].

```

**Constraints:**

- `n == workers.length`
- `m == bikes.length`
- `1 <= n <= m <= 1000`
- `workers[i].length == bikes[j].length == 2`
- `0 <= xi, yi < 1000`
- `0 <= xj, yj < 1000`
- All worker and bike locations are **unique**.

comment:

看到有bucket sort solution

分析复杂度有些麻烦

主要考察找适当的container来储蓄和更新数据，实现起来前前后后可能花了几个小时。。。

另外发现没法仔细通顺的看下去别人的答案。

```cpp
using ii=pair<int,int>;

class Solution {
public:
    vector<int> assignBikes(vector<vector<int>>& workers, vector<vector<int>>& bikes) {
        vector<set<ii>> v;//worker - sorted[(dist,bike)]
        //build v
        for(int i=0;i<workers.size();i++){
            set<ii> bikeSt;
            for(int j=0;j<bikes.size();j++){
                int d=dist(workers[i], bikes[j]);
                bikeSt.insert({d,j});
            }
            v.push_back(bikeSt);
        }
        //process
        vector<int> res(workers.size(),-1);
        unordered_set<int> toRmBikes;
        for(auto& w: workers){//execute workers.size() times
            //find min dist for w-b
            int minDist=INT_MAX, minW=-1, minB=-1;
            for(int i=0; i<v.size(); i++){
                //rm bikes if required
                while(!v[i].empty() && toRmBikes.count(v[i].begin()->second)) v[i].erase(v[i].begin());
                if(v[i].empty()) continue; //check visited
                auto [d,b] = *v[i].begin(); //min dist to bike for cur worker
                if(d<minDist){
                    minDist=d;
                    minB=b;
                    minW=i;
                }
            }
            //rm w
            v[minW].clear();
            res[minW]=minB;
            //mark b to rm
            toRmBikes.insert(minB);
        }
        return res;
    }

    int dist(vector<int>& a, vector<int>& b){
        return abs(a[0]-b[0]) + abs(a[1]-b[1]);
    }
};
```