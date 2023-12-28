# Find the Celebrity

#: 277
Difficult: Medium
Tags: DP, Graph, Greedy
link: https://leetcode.com/problems/find-the-celebrity/
Priority: Medium
Status: No Idea At First
Created time: October 16, 2022 5:09 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

Suppose you are at a party with `n` people labeled from `0` to `n - 1` and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know the celebrity, but the celebrity does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is ask questions like: "Hi, A. Do you know B?" to get information about whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function `bool knows(a, b)` that tells you whether A knows B. Implement a function `int findCelebrity(n)`. There will be exactly one celebrity if they are at the party.

Return *the celebrity's label if there is a celebrity at the party*. If there is no celebrity, return `-1`.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/01/19/g1.jpg](https://assets.leetcode.com/uploads/2022/01/19/g1.jpg)

```
Input: graph = [[1,1,0],[0,1,0],[1,1,1]]
Output: 1
Explanation: There are three persons labeled with 0, 1 and 2. graph[i][j] = 1 means person i knows person j, otherwise graph[i][j] = 0 means person i does not know person j. The celebrity is the person labeled as 1 because both 0 and 2 know him but 1 does not know anybody.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/01/19/g2.jpg](https://assets.leetcode.com/uploads/2022/01/19/g2.jpg)

```
Input: graph = [[1,0,1],[1,1,0],[0,1,1]]
Output: -1
Explanation: There is no celebrity.

```

**Constraints:**

- `n == graph.length == graph[i].length`
- `2 <= n <= 100`
- `graph[i][j]` is `0` or `1`.
- `graph[i][i] == 1`

**Follow up:** If the maximum number of allowed calls to the API `knows` is `3 * n`, could you find a solution without exceeding the maximum number of calls?

```cpp
/* The knows API is defined for you.
      bool knows(int a, int b); */

class Solution {
public:
    int findCelebrity(int n) {
        //find candidate:
        //start from 0, a konws b, then goto b, if not, check a if knows b+1
        int candidate=0;
        for(int i=1; i<n; i++){
            if(knows(candidate,i)){
                candidate=i;
            }
        } 
        if(isCelebrity(candidate,n)){
            return candidate;
        }
        return -1;
    }
    
    bool isCelebrity(int candidate, int n){
        for(int i=0; i<n; i++){
            if(candidate==i) continue;
            if(knows(candidate,i) || !knows(i,candidate)){
                return false;
            }
        }
        return true;
    }
};
```