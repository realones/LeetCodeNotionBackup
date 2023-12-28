# Container With Most Water

#: 11
Difficult: Medium
Tags: 2pt, Array, Greedy
link: https://leetcode.com/problems/container-with-most-water/
Priority: High
Status: No Idea At First, toSummarize
Created time: May 12, 2023 2:18 AM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75, LC_Top_Interview_150, wisdompeak

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return *the maximum amount of water a container can store*.

**Notice** that you may not slant the container.

**Example 1:**

![https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

```

**Example 2:**

```
Input: height = [1,1]
Output: 1

```

**Constraints:**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`

Solution new:

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n=height.size();
        int l=0, r=n-1;
        int res=0;
        while(l<r){
            int lh=height[l], rh=height[r];
            res=max(res,(r-l)*min(lh,rh));
            if(lh<rh) l++;
            else r--;
        }
        return res;
    }
}
```

Solution: todo summarize inc and dec trending loop, similar to question H-Index

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int res=0;
        int l=0,r=height.size()-1;
        while(l<r){
            int lh=height[l];
            int rh=height[r];
            res=max(res, (r-l)*min(lh,rh));
            if(lh<rh){
                for(l++;lh<rh && lh>=height[l];l++);
            }
            else{
                for(r--;lh<rh && rh>=height[r];r--);
            }
        }
        return res;
    }
};
```