# Largest Rectangle in Histogram

#: 84
Difficult: Hard
Tags: monotonic, stack
link: https://leetcode.com/problems/largest-rectangle-in-histogram/
Priority: High
Created time: September 6, 2022 6:40 PM
Last edited time: October 28, 2023 1:36 PM
practicedTimes: 1
source: jiuzhang, 算高Heap&Stack
related: Trapping Rain Water (Trapping%20Rain%20Water%20b68c807181944d70acaecbc2289e76e4.md), Maximum Score of a Good Subarra (Maximum%20Score%20of%20a%20Good%20Subarra%2091eca7c00d054e88b8b91206c322ff51.md)

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return *the area of the largest rectangle in the histogram*.

**Example 1:**

![https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

```
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

```
Input: heights = [2,4]
Output: 4

```

**Constraints:**

- `1 <= heights.length <= 105`
- `0 <= heights[i] <= 104`

我的思路：

数学归纳法类似的推导法，从第K个解法开始，推导一下(K+1)怎么扩展, 然后又将怎么影响K+2

已经有了一块rectangle区域，如果向左边或右边（因为对称，暂时只考虑右边）加一个histogram bar，将如何变化：

- 如果加一块一样高的->面积变大
- 如果加一块更高的->面积变大->同时新的高度可以产生新的最大矩阵可能性
- 如果加一块更矮的->K的高度的矩阵无法向右扩展了->需要结算了，同理，把比K+1高的都结算了

其实还是多在脑子里建模考虑如何推导和影响。

Tip：

最后设一个值为0or-1的dummy node，用来把所有的东西从stack里pop出来

这一点有点类似skyline的那个题

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> stk; //idx with height inc
        heights.push_back(0);
        heights.insert(heights.begin(),0);
        stk.push(0);
        int res=0;
        for(int i=1; i<heights.size(); i++){
            while(heights[stk.top()] > heights[i]){
                int cur = stk.top(); stk.pop();
                int h = heights[cur];
                int w = i-stk.top()-1;
                res=max(res,w*h);
            }
            stk.push(i);
        }
        return res;
    }
};
```