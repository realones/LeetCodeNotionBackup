# Asteroid Collision

#: 735
Difficult: Medium
Tags: Array, simulation, stack
link: https://leetcode.com/problems/asteroid-collision
Priority: Low
Status: checkBetterSolution
Created time: June 27, 2023 10:58 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_75
related: Robot Collisions (Robot%20Collisions%2074fd07e1831b47759a2b6291c04347b2.md)

We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Example 1:**

```
Input: asteroids = [5,10,-5]
Output: [5,10]
Explanation: The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

```

**Example 2:**

```
Input: asteroids = [8,-8]
Output: []
Explanation: The 8 and -8 collide exploding each other.

```

**Example 3:**

```
Input: asteroids = [10,2,-5]
Output: [10]
Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.

```

**Constraints:**

- `2 <= asteroids.length <= 104`
- `1000 <= asteroids[i] <= 1000`
- `asteroids[i] != 0`

Solution:

the while part can be improved, I didn’t spend lot of time to investigate how to improve it but just finish it asap, so it can be improved to be similar to 2751. Robot Collisions

```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        vector<int> res;
        stack<int> stk;
        for(auto a: asteroids){
            if(a>0) stk.push(a);
            else{
                while(!stk.empty() && stk.top()>0){
                    int top=stk.top(); stk.pop();
                    if(top>(-a)){
                        a=0;
                        stk.push(top);
                        break;
                    }
                    else if(top==-a){
                        a=0;
                        break;
                    }
                    else{
                    }
                }
                if(a<0) stk.push(a);
            }
        }
        while(!stk.empty()) res.push_back(stk.top()), stk.pop();
        reverse(res.begin(), res.end());
        return res;
    }
};

/*
stk:

if empty, put in

if -, collide with + in stk

if +, put

*/
```