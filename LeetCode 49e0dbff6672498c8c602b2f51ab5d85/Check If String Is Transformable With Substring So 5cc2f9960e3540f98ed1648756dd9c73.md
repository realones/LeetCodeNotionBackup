# Check If String Is Transformable With Substring Sort Operations

#: 1585
Difficult: Hard
Tags: Greedy, Sort, queue, string
link: https://leetcode.com/problems/check-if-string-is-transformable-with-substring-sort-operations/
Priority: High
Status: No Idea At First
Created time: June 23, 2023 4:36 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua
related: Determine if Two Strings Are Close (Determine%20if%20Two%20Strings%20Are%20Close%204fa6319cd49f40d9bfd54c2e8ea4450f.md)

Given two strings `s` and `t`, transform string `s` into string `t` using the following operation any number of times:

- Choose a **non-empty** substring in `s` and sort it in place so the characters are in **ascending order**.
    - For example, applying the operation on the underlined substring in `"14234"` results in `"12344"`.

Return `true` if *it is possible to transform `s` into `t`*. Otherwise, return `false`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

```
Input: s = "84532", t = "34852"
Output: true
Explanation: You can transform s into t using the following sort operations:
"84532" (from index 2 to 3) -> "84352"
"84352" (from index 0 to 2) -> "34852"

```

**Example 2:**

```
Input: s = "34521", t = "23415"
Output: true
Explanation: You can transform s into t using the following sort operations:
"34521" -> "23451"
"23451" -> "23415"

```

**Example 3:**

```
Input: s = "12345", t = "12435"
Output: false

```

**Constraints:**

- `s.length == t.length`
- `1 <= s.length <= 105`
- `s` and `t` consist of only digits.

```cpp
class Solution {
public:
    bool isTransformable(string s, string t) {
        //check if sort(s)==sort(t), should be covered by regular process
        //put s into dq[10]
        vector<queue<int>> idxQ(10);
        for(int i=0; i<s.size(); i++){
            idxQ[s[i]-'0'].push(i);
        }
        //process
        for(auto c:t){
            int d=c-'0';
            if(idxQ[d].empty()) return false;
            for(int sd=0; sd<d; sd++){//smaller digit
                if(!idxQ[sd].empty() && idxQ[sd].front()<idxQ[d].front())
                    return false;
            }
            idxQ[d].pop();
        }
        return true;
    }
};

/*
for multiple d_idx in s,
  put in collection[d]:
    idx 0 2 4 8 inc,
    push current from right[back]
    get/pop smallest from left[front]
so the collection should be q

for each c->d in t,
  if for ld:0,1,..,d-1 has idx:dq[ld].front in s smaller than d's idx:dq[d].front
    true->return false
    false->
      rm d from collection: dp[d].pop_front, go to next c
  return true
*/

/*
intuition: bubble sort

sort: move smaller ones to left, larger ones to right

we can always move a single digit to the left until there is a smaller one just like bubble sort by swapping adjacent elements.
e.g. [4]: 37584 -> 34758

then the problem reduced to:
can we transfrom s to t by moving smaller digits to the left?
(math, prove?)
　
84532
->
34852

================================
O(n^2)
example res=true
for each element in t
  get the position in current s (using mp<val,idx>)
  
  3 from 845[3]2 to [3]4852
  check if 3 can move in current s from idx3 to idx0
    since 8/5/4 > 3, it is possible, move it
  ->38452

  4 from 38[4]52 to 3[4]852
  check if 4 can move in current s from idx2 to idx1
    since 8 > 4, it is possible, move it
  ->34852

example res=false
for each element in t
  get the position in current s (using mp<val,idx>)
  
  4 from 123[4]5 to 12[4]35
  check if 4 can move in current s from idx3 to idx2
    since 3 !> 4, it is not possible, return false
  ->38452

================================
greedy for dups
if second 3 can be moved to posA, then first 3 must can be moved to there,
visa verse is not correct, since there can be smaller ones e.g. 2 between two 3s
================================
Create 10 queues for 0 1 2 ... 9, to store the idx of each digits in S.
We can check whether there is a any smaller digits in front of the one to be moved to the front

for each element in t
  instead of above: get the position in current s (using mp<val,idx>)
  find the first occurance of it in s - maintained by 10 queues
  can move to front?
    if there is no smaller ones in front of it

T O(n)
S O(n)
*/
```