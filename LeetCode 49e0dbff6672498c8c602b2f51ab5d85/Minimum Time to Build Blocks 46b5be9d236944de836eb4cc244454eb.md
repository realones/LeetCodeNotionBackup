# Minimum Time to Build Blocks

#: 1199
Difficult: Hard
Tags: Greedy, Huffman, Math, PQ
link: https://leetcode.com/problems/minimum-time-to-build-blocks/description/
Priority: High
Status: Dig More, No Idea At First, NotUnderstand, toRevisit
Created time: September 4, 2023 3:41 PM
Last edited time: October 17, 2023 6:24 PM
source: leetcode_daily
related: Minimum Cost to Connect Sticks (Minimum%20Cost%20to%20Connect%20Sticks%20dd61725497ef47c7b513f7a9accbe562.md)

You are given a list of blocks, where `blocks[i] = t` means that the `i`-th block needs `t` units of time to be built. A block can only be built by exactly one worker.

A worker can either split into two workers (number of workers increases by one) or build a block then go home. Both decisions cost some time.

The time cost of spliting one worker into two workers is given as an integer `split`. Note that if two workers split at the same time, they split in parallel so the cost would be `split`.

Output the minimum time needed to build all blocks.

Initially, there is only **one** worker.

**Example 1:**

```
Input: blocks = [1], split = 1
Output: 1
Explanation:We use 1 worker to build 1 block in 1 time unit.

```

**Example 2:**

```
Input: blocks = [1,2], split = 5
Output: 7
Explanation:We split the worker into 2 workers in 5 time units then assign each of them to a block so the cost is 5 + max(1, 2) = 7.

```

**Example 3:**

```
Input: blocks = [1,2,3], split = 1
Output: 4
Explanation:Split 1 worker into 2, then assign the first worker to the last block and split the second worker into 2.
Then, use the two unassigned workers to build the first two blocks.
The cost is 1 + max(3, 1 + max(1, 2)) = 4.

```

**Constraints:**

- `1 <= blocks.length <= 1000`
- `1 <= blocks[i] <= 10^5`
- `1 <= split <= 100`

comment: new algorithem i don’t know, even now sure why it works.

Solution Huffman’s Algorithm:

```cpp
class Solution {
public:
    int minBuildTime(vector<int>& blocks, int split) {
        priority_queue<int,vector<int>,greater<>> pq(blocks.begin(),blocks.end());
        while(pq.size()>1){
            int a=pq.top(); pq.pop();
            int b=pq.top(); pq.pop();
            pq.push(max(a,b)+split);
        }
        return pq.top();
    }
};

/*
Huffman's Algorithm,
put blocks in pq, 
everytime pick the smallest two, 
    operation: split worker, work on those two, push back total time into pq
repeat until there is only one element in pq, which is ans

1 3 4 7 10,  5
 
 5
1 3 ==> 8

4 7 8 10,  5

 5
4 7 ==> 12

8 10 12,  5

 5
8 10 ==> 15

12 15, 5

  5
12 15 ==> 20

20, 5

then the ans is 20

*/
```