# Find K Pairs with Smallest Sums

#: 373
Difficult: Medium
Tags: PQ
link: https://leetcode.com/problems/find-k-pairs-with-smallest-sums/
Priority: High
Status: No Idea At First, toSummarize
Created time: June 5, 2023 3:27 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

You are given two integer arrays `nums1` and `nums2` sorted in **ascending order** and an integer `k`.

Define a pair `(u, v)` which consists of one element from the first array and one element from the second array.

Return *the* `k` *pairs* `(u1, v1), (u2, v2), ..., (uk, vk)` *with the smallest sums*.

**Example 1:**

```
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]]
Explanation: The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

```

**Example 2:**

```
Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [[1,1],[1,1]]
Explanation: The first 2 pairs are returned from the sequence: [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]

```

**Example 3:**

```
Input: nums1 = [1,2], nums2 = [3], k = 3
Output: [[1,3],[2,3]]
Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]

```

**Constraints:**

- `1 <= nums1.length, nums2.length <= 105`
- `109 <= nums1[i], nums2[i] <= 109`
- `nums1` and `nums2` both are sorted in **ascending order**.
- `1 <= k <= 104`

```cpp
class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        vector<vector<int>> res;
        unordered_map<int,unordered_map<int,bool>> visited;
        int x=0,y=0;//idx for nums1 and nums2
        //tuple: val, x, y
        priority_queue<tuple<int,int,int>,vector<tuple<int,int,int>>,greater<>> pq;
        pq.emplace(nums1[0]+nums2[0],0,0);
        visited[0][0]=1;
        for(int i=0; i<k && !pq.empty(); i++){
            auto [_,x,y] = pq.top(); pq.pop();
            res.push_back({nums1[x],nums2[y]});
            if(x+1<nums1.size() && !visited[x+1][y]){
                pq.emplace(nums1[x+1]+nums2[y],x+1,y);
                visited[x+1][y]=1;
            }
            if(y+1<nums2.size() && !visited[x][y+1]){
                pq.emplace(nums1[x]+nums2[y+1],x,y+1);
                visited[x][y+1]=1;
            }
        }
        return res;
    }
};
```