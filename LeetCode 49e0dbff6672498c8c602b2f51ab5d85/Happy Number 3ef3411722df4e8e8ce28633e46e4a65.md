# Happy Number

#: 202
Difficult: Easy
Tags: Hash, circle
link: https://leetcode.com/problems/happy-number/
Priority: Medium
Created time: June 2, 2023 10:52 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.

Return `true` *if* `n` *is a happy number, and* `false` *if not*.

**Example 1:**

```
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

```

**Example 2:**

```
Input: n = 2
Output: false

```

**Constraints:**

- `1 <= n <= 231 - 1`

Solution 1:

```cpp
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> visited;
        while(n!=1){
            if(visited.count(n)) return false;
            visited.insert(n);
            int nxt=0;
            while(n){
                nxt+=pow(n%10,2);
                n/=10;
            }
            n=nxt;
        }
        return true;
    }
};
```

Solution 2:

```cpp
class Solution {
public:
    bool isHappy(int n) {
        if(n<=0) return false;
        int slow=next(n);
        int fast=next(next(n));
        while(slow!=fast){
            slow=next(slow);
            fast=next(next(fast));
        }
        return slow==1;
    }
    int next(int n){
        int res=0;
        for(;n;n/=10){
            res+=pow(n%10,2);
        }
        return res;
    }
};
```