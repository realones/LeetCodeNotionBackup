# Valid Perfect Square

#: 367
Difficult: Easy
Tags: BS_Val, Math
link: https://leetcode.com/problems/valid-perfect-square/description/
Priority: Low
Created time: December 3, 2023 4:45 PM
Last edited time: December 3, 2023 4:46 PM
source: facebookFreq

Given a positive integer num, return `true` *if* `num` *is a perfect square or* `false` *otherwise*.

A **perfect square** is an integer that is the square of an integer. In other words, it is the product of some integer with itself.

You must not use any built-in library function, such as `sqrt`.

**Example 1:**

```
Input: num = 16
Output: true
Explanation: We return true because 4 * 4 = 16 and 4 is an integer.

```

**Example 2:**

```
Input: num = 14
Output: false
Explanation: We return false because 3.742 * 3.742 = 14 and 3.742 is not an integer.

```

**Constraints:**

- `1 <= num <= 231 - 1`

```cpp
using ll=long long;

class Solution {
public:
    bool isPerfectSquare(int num) {
        ll l=0,r=num;
        // SSSSSSSELLLLLL
        while(l<r){
            ll m=l+(r-l+1)/2;
            if(m*m > (ll)num) {r=m-1;}//if not qualify
            else {l=m;}
        }
        return r*r==num;
    }
};
```