# N-Queens II

#: 52
Difficult: Hard
Tags: BackTracking
link: https://leetcode.com/problems/n-queens-ii/
Priority: Low
Created time: June 5, 2023 1:40 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *the number of distinct solutions to the **n-queens puzzle***.

**Example 1:**

![https://assets.leetcode.com/uploads/2020/11/13/queens.jpg](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
Input: n = 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown.

```

**Example 2:**

```
Input: n = 1
Output: 1

```

**Constraints:**

- `1 <= n <= 9`

```cpp
class Solution {
public:
    vector<int> visited;//can be removed and check in valid method
    vector<int> nums;
    int res=0;
    int n;
    
    int totalNQueens(int n) {
        this->n=n;
        visited.resize(n,0);
        backtracking(n);
        return res;
    }
    
    void backtracking(int k){
        if(k==0){res+=1; return;}
        for(int i=0; i<n; i++){
            if(visited[i]) continue;
            if(!valid(i)) continue;
            visited[i]=1;
            nums.push_back(i);
            backtracking(k-1);
            nums.pop_back();
            visited[i]=0;
        }
    }
    
    bool valid(int can){
        int n=nums.size();
        for(int i=0;i<n;i++){
            if(nums[i]-i == can-n) return false;
            if(nums[i]+i == can+n) return false;
        }
        return true;
    }
};
/*
can be converted to find number of permutation of 0...n-1 for certain criteria
e.g. example1 can be 1,3,0,2

criteria:
    nums[i]-i != nums[j]-j
    nums[i]+i != nums[j]+j
*/
```