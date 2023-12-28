# Maximal Rectangle

#: 85
Difficult: Hard
Tags: DP, Matrix, monotonic, stack
link: https://leetcode.com/problems/maximal-rectangle/
Priority: High
Status: More Solution
Created time: September 6, 2022 6:42 PM
Last edited time: October 28, 2023 1:37 PM
practicedTimes: 1
source: jiuzhang, 算高DP上, 算高Heap&Stack

Given a `rows x cols` binary `matrix` filled with `0`'s and `1`'s, find the largest rectangle containing only `1`'s and return *its area*.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg)

```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 6
Explanation: The maximal rectangle is shown in the above picture.

```

**Example 2:**

```
Input: matrix = [["0"]]
Output: 0

```

**Example 3:**

```
Input: matrix = [["1"]]
Output: 1

```

**Constraints:**

- `rows == matrix.length`
- `cols == matrix[i].length`
- `1 <= row, cols <= 200`
- `matrix[i][j]` is `'0'` or `'1'`.

推导思路：

一步步的走

1. 暴力解法，找所有的可能性
2. 暴力解法都重复计算了什么：
    1. 不管矩阵是向下，向右，还是向右下扩展，都重复计算了。
        1. 尝试减少重复计算
            1. 向右下扩展，不好办，找一个反例即可，所以不得不加限制条件，增加时间/空间复杂度
            2. 然后考虑向右扩展，发现不好想，改想向下扩展
                1. 需要给定底边，找每一个岛屿的底边，发现底边要一层层的便利，思维改为向上扩展，这样就类似于largest-rectangle-in-histogram
                2.  同时发现，一层层的找底边，正好可以build histogram的height

1. Monotonous stack
2. DP-maximal square like
3. DP-逐层，ps: 逐层比逐个要难想一些

see next solution for improvement

```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int m=matrix.size(); if(m==0) {return 0;}
        int n=matrix[0].size(); if(n==0) {return 0;}
        
        vector<vector<int>> h(m,vector<int>(n,0));
        vector<vector<int>> l(m,vector<int>(n,0));
        vector<vector<int>> r(m,vector<int>(n,0));
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]=='0') h[i][j]=0;
                else{
                    h[i][j]+=((i==0)?0:h[i-1][j])+1;
                }
            }
        }
        
        for(int i=0;i<m;i++){
            int ll=0;
            for(int j=0;j<n;j++){
                if(matrix[i][j]=='0') {
                    l[i][j]=0;
                    ll=j+1;
                }
                else{
                    l[i][j]=max( (i==0)?0:l[i-1][j], ll );
                }
            }
        }
        
        for(int i=0;i<m;i++){
            int rr=n-1;
            for(int j=n-1;j>=0;j--){
                if(matrix[i][j]=='0') {
                    r[i][j]=n-1;
                    rr=j-1;
                }
                else{
                    r[i][j]=min( (i==0)?n-1:r[i-1][j], rr );
                }
            }
        }
        
        int res=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                res=max(res, (r[i][j]-l[i][j]+1)*h[i][j]);
            }
        }

        return res;
    }
};
```

```cpp
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int m=matrix.size(); if(m==0) return 0;
        int n=matrix[0].size(); if(n==0) return 0;
        
        normalize(matrix);
        
        int res=0;
        for(int i=1;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==0) continue;
                else{
                    matrix[i][j]+=matrix[i-1][j];
                }
            }
        }
        
        for(auto &r: matrix){
            res=max(res,maxRect(r));
        }
        
        return res;
    }
    
    int maxRect(vector<char>& r){
        int res=0;
        stack<int> stk;//maintain an increasing stack
        r.push_back(-1);
        for(int i=0;i<r.size();i++){
            while(!stk.empty() && r[stk.top()]>r[i]){
                int h=r[stk.top()]; stk.pop();
                int w=i - (!stk.empty() ? stk.top()+1 : 0 );
                res=max(res,h*w);
            }
            
            stk.push(i);
        }
        
        return res;
    }
    
    void normalize(vector<vector<char>>& matrix){
        for(auto& r:matrix){
            for(auto& e: r){
                e-='0';
            }
        }
    }
    
};

//================================== 'New Comment                ' =====================================
// // DP solution
// // calculate subquestions line by line, same as monotonous stack solution
// // T: O(m*n)
// class Solution {
// public:
//     int maximalRectangle(vector<vector<char> > &matrix) {
//         if(matrix.empty()) return 0;
//         const int m = matrix.size();
//         const int n = matrix[0].size();
//         int left[n], right[n], height[n];
//         fill_n(left,n,0); fill_n(right,n,n); fill_n(height,n,0);
//         int maxA = 0;
//         for(int i=0; i<m; i++) {
//             int cur_left=0, cur_right=n;
//             // compute height (can do this from either side)
//             // line by line, get the height of continues 1
//             // h(i,j) = h(i-1,j) + ([i,j]==1)?1:0 
//             for(int j=0; j<n; j++) { 
//                 if(matrix[i][j]=='1') height[j]++;
//                 else height[j]=0;
//             }
//             // compute left (from left to right)
//             // l(i,j) = min ( l(i-1,j), fl="coorrdinate of last continues 1 to the left" )
//             // fl: enhance to O(1)
//             for(int j=0; j<n; j++) { 
//                 if(matrix[i][j]=='1') left[j]=max(left[j],cur_left);
//                 else {left[j]=0; cur_left=j+1;}
//             }
//             // compute right (from right to left)
//             // r(i,j) = min ( r(i-1,j), fr="coorrdinate of last continues 1 to the right" ) 
//             // fr: enhance to O(1)
//             for(int j=n-1; j>=0; j--) {
//                 if(matrix[i][j]=='1') right[j]=min(right[j],cur_right);
//                 else {right[j]=n; cur_right=j;}
//             }
//             // compute the area of rectangle (can do this from either side)
//             for(int j=0; j<n; j++)
//                 maxA = max(maxA,(right[j]-left[j])*height[j]);
//         }
//         return maxA;
//     }
// };
```