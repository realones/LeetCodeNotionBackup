# Implement Rand10() Using Rand7()

#: 470
Difficult: Medium
Tags: Math, Probability, Random, rejection_sampling
link: https://leetcode.com/problems/implement-rand10-using-rand7/
Priority: High
Status: More Solution, No Idea At First
Created time: September 11, 2023 4:53 PM
Last edited time: October 12, 2023 2:56 PM

Given the **API** `rand7()` that generates a uniform random integer in the range `[1, 7]`, write a function `rand10()` that generates a uniform random integer in the range `[1, 10]`. You can only call the API `rand7()`, and you shouldn't call any other API. Please **do not** use a language's built-in random API.

Each test case will have one **internal** argument `n`, the number of times that your implemented function `rand10()` will be called while testing. Note that this is **not an argument** passed to `rand10()`.

**Example 1:**

```
Input: n = 1
Output: [2]

```

**Example 2:**

```
Input: n = 2
Output: [2,8]

```

**Example 3:**

```
Input: n = 3
Output: [3,8,10]

```

**Constraints:**

- `1 <= n <= 105`

**Follow up:**

- What is the [expected value](https://en.wikipedia.org/wiki/Expected_value) for the number of calls to `rand7()` function?
- Could you minimize the number of calls to `rand7()`?

Solution 1: 

```cpp
// The rand7() API is already defined for you.
// int rand7();
// @return a random integer in the range 1 to 7

class Solution {
public:
    int rand10() {
        int r=rand7();
        int c=rand7();
        int x=(r-1)*7+c-1;
        if(x<40) return x%10 + 1;
        return rand10();
    }
};
/*
rejection sampling
rand7 twice = 49
r*7+c+1
if 1~40, just %10
if 41~49, redo
*/
```

Solution 2:

```cpp
class Solution {
public:
    int rand10() {
        int row, col, idx;
        do {
            row = rand7();
            col = rand7();
            idx = col + (row - 1) * 7;
        } while (idx > 40);
        return 1 + (idx - 1) % 10;
    }
};
```

Solution 3 ****Utilizing out-of-range samples****:

todo