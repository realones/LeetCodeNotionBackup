# Find Building Where Alice and Bob Can Meet

#: 2940
Difficult: Hard
Tags: Array, OrderMap, SegmentTree_val, monotonic, stack
link: https://leetcode.com/problems/find-building-where-alice-and-bob-can-meet/description/
Priority: High
Status: Dig More, HardToImplement, More Solution, No Idea At First, NotUnderstand, toSummarize
Created time: November 19, 2023 12:46 AM
Last edited time: December 14, 2023 2:05 AM
source: LC_Contests
related: Count the Number of K-Big Indices (Count%20the%20Number%20of%20K-Big%20Indices%202889b3a80dcb47b78932dcf715269040.md), 132 Pattern (132%20Pattern%2095e3fe9db4714ff9959e5740cefb4926.md), Reverse Pairs (Reverse%20Pairs%20253d0403dba84fc2857f5c4546ebdd39.md)

You are given a **0-indexed** array `heights` of positive integers, where `heights[i]` represents the height of the `ith` building.

If a person is in building `i`, they can move to any other building `j` if and only if `i < j` and `heights[i] < heights[j]`.

You are also given another array `queries` where `queries[i] = [ai, bi]`. On the `ith` query, Alice is in building `ai` while Bob is in building `bi`.

Return *an array* `ans` *where* `ans[i]` *is **the index of the leftmost building** where Alice and Bob can meet on the* `ith` *query*. *If Alice and Bob cannot move to a common building on query* `i`, *set* `ans[i]` *to* `-1`.

**Example 1:**

```
Input: heights = [6,4,8,5,2,7], queries = [[0,1],[0,3],[2,4],[3,4],[2,2]]
Output: [2,5,-1,5,2]
Explanation: In the first query, Alice and Bob can move to building 2 since heights[0] < heights[2] and heights[1] < heights[2].
In the second query, Alice and Bob can move to building 5 since heights[0] < heights[5] and heights[3] < heights[5].
In the third query, Alice cannot meet Bob since Alice cannot move to any other building.
In the fourth query, Alice and Bob can move to building 5 since heights[3] < heights[5] and heights[4] < heights[5].
In the fifth query, Alice and Bob are already in the same building.
For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet.
For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.

```

**Example 2:**

```
Input: heights = [5,3,8,2,6,1,4,6], queries = [[0,7],[3,5],[5,2],[3,0],[1,6]]
Output: [7,6,-1,4,6]
Explanation: In the first query, Alice can directly move to Bob's building since heights[0] < heights[7].
In the second query, Alice and Bob can move to building 6 since heights[3] < heights[6] and heights[5] < heights[6].
In the third query, Alice cannot meet Bob since Bob cannot move to any other building.
In the fourth query, Alice and Bob can move to building 4 since heights[3] < heights[4] and heights[0] < heights[4].
In the fifth query, Alice can directly move to Bob's building since heights[1] < heights[6].
For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet.
For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.

```

**Constraints:**

- `1 <= heights.length <= 5 * 104`
- `1 <= heights[i] <= 109`
- `1 <= queries.length <= 5 * 104`
- `queries[i] = [ai, bi]`
- `0 <= ai, bi <= heights.length - 1`

todo: check lee215

to summarize:

seg_tree_val → orderMap → mono stk when idx mono change and check on values

Solution votrubac:

the implementation is very hard to me, even though i understand the algorithm, so marked as not understand,

```cpp
using abi = tuple<int,int,int>;

class Solution {
public:
    vector<int> leftmostBuildingQueries(vector<int>& heights, vector<vector<int>>& qs) {
        vector<int> res(qs.size(),-1);
        vector<abi> qs2;//a,b,i
        for(int i=0; i<qs.size(); i++) {
            //1. swap idx in each query so that a<=b
            sort(qs[i].begin(), qs[i].end());
            //2. exclude obvious cases when a==b or heights[a]<heights[b], -> their res is b
            if(qs[i][0]==qs[i][1] || heights[qs[i][0]]<heights[qs[i][1]]) 
                res[i]=qs[i][1]; 
            //3. preprocess for dec sort remaining qs by b  => qs+idx
            else{
                qs2.push_back({qs[i][0],qs[i][1],i});
            }
        }
        //3. sort by b, dec
        sort(qs2.begin(), qs2.end(), [&](abi& x, abi& y){return get<1>(x) > get<1>(y);});

        //4. for each b, maintain a mono stk of heigths for range (b,n)
        vector<int> stk; // (imaging stk.top at left, since b is mono dec)
        int j=heights.size()-1;//push all not_pushed j>b into stk
        for(auto [a,b,i]: qs2){//b is dec idx from right to left on heights
            // pop all elem <= b
            // stk.top is b, which is the smallest one
            while(j>b){
                while(!stk.empty() && heights[stk.back()] <= heights[j]) stk.pop_back();
                stk.push_back(j);
                j--;
            }
            //5. bs the mono stk for the first height > a, reverse bs since stk is reversed
            auto iter=upper_bound(stk.rbegin(), stk.rend() , a, [&](int i, int j){
                return heights[i] < heights[j];
            });
            if(iter==stk.rend()) res[i]=-1;
            else res[i]=*iter;
        }
        return res;
    }
};
/*
heights: 
    val: 1~1e9
    len: 1~5e4
queries:  
    val: 0~heights.len-1
    len: 1~5e4
================================
idea1:
    w or w/o preprocessing, find nextLarger of x in range (i,n-1), x=max(query vals), i=max(query idx. )
    in O(1) or O(logN)
================================
idea votrubac:

intui: exploits the fact that we know all queries in advance -> we can process them in any order we want
1. swap idx in each query so that a<=b
2. exclude obvious cases when a==b or heights[a]<heights[b], -> their res is b
3. dec sort remaining queries by b
    ================================
    at this time:
    idea1:
        find nextLarger of x in range (b,n-1), x=max(query vals), b mono dec, so right to left
            in O(1) or O(logN)
        O(logN)
        -> segTree_val, since idx mono dec and check values
            build and check for each elem
        -> map<val,idx>
        otherwise, see below
        O(1)
    ================================
4. for each b, maintain a mono stk of heigths for range (b,n)
    pop elems < b
    all elem >= b
    stk.top is b, which is the smallest one
    (imaging stk.top at left, since b is mono dec)
5. bs the mono stk for the first height > a
*/
```