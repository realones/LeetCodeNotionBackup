# Max Value of Equation

#: 1499
Difficult: Hard
Tags: Array, Heap, PQ, Sliding Window, deque, monotonic
link: https://leetcode.com/problems/max-value-of-equation/
Priority: High
Status: Dig More, HardToImplement, No Idea At First, toSummarize
Created time: June 23, 2023 4:36 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua

You are given an array `points` containing the coordinates of points on a 2D plane, sorted by the x-values, where `points[i] = [xi, yi]` such that `xi < xj` for all `1 <= i < j <= points.length`. You are also given an integer `k`.

Return *the maximum value of the equation* `yi + yj + |xi - xj|` where `|xi - xj| <= k` and `1 <= i < j <= points.length`.

It is guaranteed that there exists at least one pair of points that satisfy the constraint `|xi - xj| <= k`.

**Example 1:**

```
Input: points = [[1,3],[2,0],[5,10],[6,-10]], k = 1
Output: 4
Explanation: The first two points satisfy the condition |xi - xj| <= 1 and if we calculate the equation we get 3 + 0 + |1 - 2| = 4. Third and fourth points also satisfy the condition and give a value of 10 + -10 + |5 - 6| = 1.
No other pairs satisfy the condition, so we return the max of 4 and 1.

```

**Example 2:**

```
Input: points = [[0,0],[3,0],[9,2]], k = 3
Output: 3
Explanation:Only the first two points have an absolute difference of 3 or less in the x-values, and give the value of 0 + 0 + |0 - 3| = 3.

```

**Constraints:**

- `2 <= points.length <= 105`
- `points[i].length == 2`
- `108 <= xi, yi <= 108`
- `0 <= k <= 2 * 108`
- `xi < xj` for all `1 <= i < j <= points.length`
- `xi` form a strictly increasing sequence.

Solution PQ 1: correct

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) {
        int res=INT_MIN;
        priority_queue<ii> pq;//{y-x,x}
        for(auto& p: points){
            int x=p[0],y=p[1];
            while(!pq.empty() && x-pq.top().second > k) pq.pop();
            if(!pq.empty()) res=max(res,pq.top().first+x+y);
            pq.emplace(y-x,x);
        }
        return res;
    }
};
```

Solution Mono deque: O(n)

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) {
        int res=INT_MIN;
        deque<ii> q; //{y-x,x} , dec on y-x 
        for(auto p: points){
            int x=p[0],y=p[1];
            //pop l
            while(!q.empty() && x - q.front().second > k)
                q.pop_front();
            //update res
            if(!q.empty())
                res=max(res,x+y+q.front().first);
            //add r
            while(!q.empty() && y-x >= q.back().first)
                q.pop_back();
            q.emplace_back(y-x, x);
        }
        return res;
    }
};
```

================================
Followings are some incorrect result, since y-x is not sorted by element idx, so need one more dimension for idx like above.

Solution multiset: O(nlogn) - Incorrect

```cpp
class Solution {
public:
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) {
        int n=points.size();
        int res=INT_MIN;
        multiset<int> st;
        st.insert(points[0][1]-points[0][0]);//y0-x0
        for(int l=0,r=1; r<n; r++){
            int xr=points[r][0],yr=points[r][1];
            while(points[r][0] - points[l][0]>k){
                int xl=points[l][0], yl=points[l][1];
                if(!st.empty() && st.count(yl-xl)) st.erase(st.find(yl-xl));
                l++;
            }
            res=max(res,*st.rbegin()+xr+yr);
            st.insert(yr-xr);
        }
        return res;
    }
}
```

Solution PQ: O(nlogn)  - Incorrect

```cpp
class Solution {
public:
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) {
        int n=points.size();
        int res=INT_MIN;
        priority_queue<int> pq;
        pq.push(points[0][1]-points[0][0]);//y0-x0
        for(int l=0,r=1; r<n; r++){
            int xr=points[r][0],yr=points[r][1];
            while(points[r][0] - points[l][0]>k){
                int xl=points[l][0], yl=points[l][1];
                if(!pq.empty() && pq.top()==yl-xl) pq.pop();
                l++;
            }
            res=max(res,pq.top()+xr+yr);
            pq.push(yr-xr);
        }
        return res;
    }
};
```

Solution Deque: O(n)  - Incorrect

```cpp
class Solution {
public:
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) {
        int n=points.size();
        int res=INT_MIN;
        deque<int> q;
        q.push_front(points[0][1]-points[0][0]);//y-x
        for(int l=0,r=1; r<n; r++){
            //while invalid with new R, pop l
            while(points[r][0]-points[l][0]>k) {
                if(!q.empty() && q.back()==points[l][1]-points[l][0]) q.pop_back();
                l++;
            }
            //push r
            while(!q.empty() && q.front()<points[r][1]-points[r][0]) q.pop_front();
            q.push_front(points[r][1]-points[r][0]);//y-x
            //update res
            res=max(res, points[r][0]+points[r][1] + q.back());
        }
        return res;
    }
};

/*
sliding window with max pos diff(xr-xl) = k
find max: yl+yr+ |xl-xr| 
= -xl+yl + xr+yr
    r: fixed

mon dec dq to maintain the (y-x) max val on the back
*/
```