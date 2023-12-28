# Friends Of Appropriate Ages

#: 825
Difficult: Medium
Tags: 2pt, Array, BS_todo, Sort
link: https://leetcode.com/problems/friends-of-appropriate-ages/description/
Priority: High
Status: More Solution, No Idea At First
Created time: December 8, 2023 1:01 PM
Last edited time: December 8, 2023 1:02 PM
source: facebookFreq

There are `n` persons on a social media website. You are given an integer array `ages` where `ages[i]` is the age of the `ith` person.

A Person `x` will not send a friend request to a person `y` (`x != y`) if any of the following conditions is true:

- `age[y] <= 0.5 * age+ 7`
- `age[y] > age[x]`
- `age[y] > 100 && age< 100`

Otherwise, `x` will send a friend request to `y`.

Note that if `x` sends a request to `y`, `y` will not necessarily send a request to `x`. Also, a person will not send a friend request to themself.

Return *the total number of friend requests made*.

**Example 1:**

```
Input: ages = [16,16]
Output: 2
Explanation: 2 people friend request each other.

```

**Example 2:**

```
Input: ages = [16,17,18]
Output: 2
Explanation: Friend requests are made 17 -> 16, 18 -> 17.

```

**Example 3:**

```
Input: ages = [20,30,100,110,120]
Output: 3
Explanation: Friend requests are made 110 -> 100, 120 -> 110, 120 -> 100.

```

**Constraints:**

- `n == ages.length`
- `1 <= n <= 2 * 104`
- `1 <= ages[i] <= 120`

```cpp
//from lee215
class Solution {
public:
    int numFriendRequests(vector<int>& ages) {
        unordered_map<int, int> count;
        for (int &age : ages)
            count[age]++;
        int res = 0;
        for (auto &a : count)
            for (auto &b : count)
                if (request(a.first, b.first))
                    res += a.second * (b.second - (a.first == b.first ? 1 : 0));
        return res;
    }

    bool request(int a, int b) {
        return !(b <= 0.5 * a + 7 || b > a || (b > 100 && a < 100));
    }
};

/*
request(a,b) = !(c1||c2||c3), no seq
return how many request==true
================================
ages:
  len: 1~2e4
  val: 1~120
================================
val is limited
mp[val]=cnt
for [b,cnt] from mp O(valRange)
  for [a,cnt] from mp O(valRange)
    check valid(a,b)
      isValid,
        a==b
          cnt[a]*(cnt[b]-1)
        a!=b
          cnt[a]*cnt[b]
      notValid, skip
*/
```