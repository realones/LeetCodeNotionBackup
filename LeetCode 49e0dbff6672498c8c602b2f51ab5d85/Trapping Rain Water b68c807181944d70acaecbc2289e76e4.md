# Trapping Rain Water

#: 42
Difficult: Hard
Tags: 2pt_DiffDir, Array, DP, Prefix, monotonic, stack
link: https://leetcode.com/problems/trapping-rain-water/
Priority: Medium
Created time: August 31, 2022 12:58 PM
Last edited time: October 28, 2023 12:56 PM
practicedTimes: 1
source: HuaHua, LC_Top_Interview_150, jiuzhang, 算高Heap&Stack
related: Largest Rectangle in Histogram (Largest%20Rectangle%20in%20Histogram%20ac2ea76b0a0645e1b28ad0283e4f7fee.md)

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

```

**Example 2:**

```
Input: height = [4,2,0,3,2,5]
Output: 9

```

**Constraints:**

- `n == height.length`
- `1 <= n <= 2 * 104`
- `0 <= height[i] <= 105`

Solution 2pt

```cpp
class Solution {
public:
    /*
     * @param heights: a list of integers
     * @return: a integer
     */
    int trap(vector<int> &heights) {
        // write your code here
        if(heights.size()<3) {return 0;}
        
        int left = 0, right = heights.size() - 1; 
        int res = 0;
        
        int leftheight=heights[left];
        int rightheight=heights[right];
        
        while(left<right){
            //左侧右移
            if(leftheight < rightheight) {
                left++;
                if(leftheight > heights[left]){
                    res+=(leftheight - heights[left]);
                }
                else{
                    leftheight = heights[left];
                }
            }
            //右侧左移
            else{
                right--;
                if(rightheight > heights[right]){
                    res+=rightheight-heights[right];
                }
                else{
                    rightheight=heights[right];
                }
            }
        }
        
        return res;
    }
};
```

Solution prefix max (dp)

```cpp
class Solution {
public:
    int trap(vector<int>& nums) {
        int n=nums.size();
        nums.push_back(0);
        nums.insert(nums.begin(),0);
        vector<int> l(n+2,0);//max val seems so far from front to cur
        for(int i=1;i<n+2;i++){
            l[i]=max(l[i-1],nums[i]);
        }
        vector<int> r(n+2,0);//max val seems so far from back to cur
        for(int i=n;i>=0;i--){
            r[i]=max(r[i+1],nums[i]);
        }
        //process
        int res=0;
        for(int i=1; i<n+1; i++){
            int h=min(l[i-1],r[i+1]) - nums[i];
            if(h>0) res+=h;
        }
        return res;
    }
};

```

Solution mono stk:

```cpp
class Solution {
public:
    int trap(vector<int>& nums) {
        int n=nums.size();// if(n==0) {return 0;}
        stack<int> stk;//idx, dec
        int res=0;
        for(int i=0;i<n;i++){
            while(!stk.empty() && nums[stk.top()]<nums[i]){
                int tmp=stk.top(); stk.pop();
                if(stk.empty()) break;
                int l=stk.top();
                int r=i;
                int h=min(nums[l],nums[r])-nums[tmp];
                res+=(r-l-1)*h;
            }
            stk.push(i);
        }
        return res;
    }
};

/*
find NLE, dec stk
|          
|       |  
|   |   |  
|   | | | |
| | | | | |
*/
```