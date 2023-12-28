# The Skyline Problem

#: 218
Difficult: Hard
Tags: Geometry, OrderMap, PQ, Sort, Sweep-Line, Tree
link: https://leetcode.com/problems/the-skyline-problem/description/
Priority: High
Created time: August 15, 2022 9:49 PM
Last edited time: October 21, 2023 9:48 PM
practicedTimes: 1
source: HuaHua, jiuzhang, 算初Hash&Heap, 算高BS&SwipeLine

A city's **skyline** is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return *the **skyline** formed by these buildings collectively*.

The geometric information of each building is given in the array `buildings` where `buildings[i] = [lefti, righti, heighti]`:

- `lefti` is the x coordinate of the left edge of the `ith` building.
- `righti` is the x coordinate of the right edge of the `ith` building.
- `heighti` is the height of the `ith` building.

You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height `0`.

The **skyline** should be represented as a list of "key points" **sorted by their x-coordinate** in the form `[[x1,y1],[x2,y2],...]`. Each key point is the left endpoint of some horizontal segment in the skyline except the last point in the list, which always has a y-coordinate `0` and is used to mark the skyline's termination where the rightmost building ends. Any ground between the leftmost and rightmost buildings should be part of the skyline's contour.

**Note:** There must be no consecutive horizontal lines of equal height in the output skyline. For instance, `[...,[2 3],[4 5],[7 5],[11 5],[12 7],...]` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: `[...,[2 3],[4 5],[12 7],...]`

**Example 1:**

![https://assets.leetcode.com/uploads/2020/12/01/merged.jpg](https://assets.leetcode.com/uploads/2020/12/01/merged.jpg)

```
Input: buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
Output: [[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]
Explanation:
Figure A shows the buildings of the input.
Figure B shows the skyline formed by those buildings. The red points in figure B represent the key points in the output list.

```

**Example 2:**

```
Input: buildings = [[0,2,3],[2,5,3]]
Output: [[0,3],[5,0]]

```

**Constraints:**

- `1 <= buildings.length <= 104`
- `0 <= lefti < righti <= 231 - 1`
- `1 <= heighti <= 231 - 1`
- `buildings` is sorted by `lefti` in non-decreasing order.

```cpp
bool cmp(const vector<int> &x, const vector<int> &y) {
    int x_idx=x[0],x_h=x[1],x_isLeft=x[2];
    int y_idx=y[0],y_h=y[1],y_isLeft=y[2];
    if(x_idx==y_idx){
        if(x_isLeft && y_isLeft){
            return x_h>y_h;
        }
        else if(!x_isLeft && !y_isLeft){
            return x_h<y_h;
        }
        else{ //if one is left, and another one is right, we extend it instead of cut into two
            return x_isLeft>y_isLeft;
        }
    }
    else{return x_idx<y_idx;}
}

class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        //input: ary of l,r,h
        //output: ary of [i,h] , the end is right endpoints with h=0
        //
        //split segments: line: x,h,isL
        //only cur heighest draw, so pq
        //support remove, so mulit_tree_set
        //draw and think, 
        //  push later/pop first, if cur > top, put into res (idx,curtop)
        vector<vector<int>> res;
        int n=buildings.size(); if(n==0) return res;
        vector<vector<int>> edges;
        for(auto building: buildings){
            edges.push_back({building[0],building[2],1});//left
            edges.push_back({building[1],building[2],0});//right
        }
        sort(edges.begin(),edges.end(),cmp);

        multiset<int> heights={0};
        for(auto e: edges){
            int idx=e[0], h=e[1], isLeft=e[2];
            int last_top=*heights.rbegin();
            if(isLeft){
                heights.insert(h);
            }
            else{
                heights.erase(heights.find(h));
            }
            int top=*heights.rbegin();
            if(top!=last_top){
                res.push_back({idx,top});
            }
        }
        return res;
    }
};
```

```cpp
class Solution {
public:
    static bool cmp(const tuple<int,int,int> &x, const tuple<int,int,int> &y) {
        auto [i1,h1,isLeft1] = x;
        auto [i2,h2,isLeft2] = y;
        if(i1!=i2) return i1<i2;
        if(isLeft1 && isLeft2){return h1>h2;}
        else if(!isLeft1 && !isLeft2){return h1<h2;}
        else{return isLeft1>isLeft2;}
    }

    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        //build edges
        vector<tuple<int,int,bool>> edges;//idx, h, isLeft
        for(int i=0; i<buildings.size(); i++){
            int l=buildings[i][0];
            int r=buildings[i][1];
            int h=buildings[i][2];
            edges.push_back({l,h,1});
            edges.push_back({r,h,0});
        }
        sort(edges.begin(), edges.end(), cmp);
        //process
        multiset<int> st;//height set
        st.insert(0);
        vector<vector<int>> res;
        for(auto [i,h,isLeft]: edges){
            int last_top=*st.rbegin();
            if(isLeft){ st.insert(h); }
            else{ st.erase(st.find(h)); }
            int top=*st.rbegin();
            if(top!=last_top) res.push_back({i,top});
        }
        return res;
    }
};
```