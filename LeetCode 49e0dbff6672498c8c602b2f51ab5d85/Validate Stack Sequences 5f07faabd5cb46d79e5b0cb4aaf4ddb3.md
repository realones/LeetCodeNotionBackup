# Validate Stack Sequences

#: 946
Difficult: Medium
Tags: Array, simulation, stack
link: https://leetcode.com/problems/validate-stack-sequences
Priority: Medium
Status: No Idea At First
Created time: November 17, 2023 1:28 AM
Last edited time: November 17, 2023 10:20 AM
practicedTimes: 1
source: ticktokFreq

Given two integer arrays `pushed` and `popped` each with distinct values, return `true` *if this could have been the result of a sequence of push and pop operations on an initially empty stack, or* `false` *otherwise.*

**Example 1:**

```
Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
Output: true
Explanation: We might do the following sequence:
push(1), push(2), push(3), push(4),
pop() -> 4,
push(5),
pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1

```

**Example 2:**

```
Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
Output: false
Explanation: 1 cannot be popped before 2.

```

**Constraints:**

- `1 <= pushed.length <= 1000`
- `0 <= pushed[i] <= 1000`
- All the elements of `pushed` are **unique**.
- `popped.length == pushed.length`
- `popped` is a permutation of `pushed`.

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        int n=pushed.size();
        stack<int> stk;
        int j=0;
        for(int i=0; i<n; i++){
            stk.push(pushed[i]);
            while(!stk.empty() && stk.top()==popped[j]) {stk.pop(); j++;}
        }
        return stk.empty();
    }
};
/*
len: 1000
input valid
================================

*/
```