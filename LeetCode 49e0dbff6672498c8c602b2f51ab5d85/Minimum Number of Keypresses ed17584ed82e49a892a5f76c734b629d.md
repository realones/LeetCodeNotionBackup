# Minimum Number of Keypresses

#: 2268
Difficult: Medium
Tags: Array, Counting, Greedy, Sort, string
link: https://leetcode.com/problems/minimum-number-of-keypresses/
Priority: Low
Created time: November 15, 2023 6:41 PM
Last edited time: November 17, 2023 1:26 AM
practicedTimes: 1
source: AmazonFreq

You have a keypad with `9` buttons, numbered from `1` to `9`, each mapped to lowercase English letters. You can choose which characters each button is matched to as long as:

- All 26 lowercase English letters are mapped to.
- Each character is mapped to by **exactly** `1` button.
- Each button maps to **at most** `3` characters.

To type the first character matched to a button, you press the button once. To type the second character, you press the button twice, and so on.

Given a string `s`, return *the **minimum** number of keypresses needed to type* `s` *using your keypad.*

**Note** that the characters mapped to by each button, and the order they are mapped in cannot be changed.

**Example 1:**

![https://assets.leetcode.com/uploads/2022/05/05/image-20220505184346-1.png](https://assets.leetcode.com/uploads/2022/05/05/image-20220505184346-1.png)

```
Input: s = "apple"
Output: 5
Explanation: One optimal way to setup your keypad is shown above.
Type 'a' by pressing button 1 once.
Type 'p' by pressing button 6 once.
Type 'p' by pressing button 6 once.
Type 'l' by pressing button 5 once.
Type 'e' by pressing button 3 once.
A total of 5 button presses are needed, so return 5.

```

**Example 2:**

![https://assets.leetcode.com/uploads/2022/05/05/image-20220505203823-1.png](https://assets.leetcode.com/uploads/2022/05/05/image-20220505203823-1.png)

```
Input: s = "abcdefghijkl"
Output: 15
Explanation: One optimal way to setup your keypad is shown above.
The letters 'a' to 'i' can each be typed by pressing a button once.
Type 'j' by pressing button 1 twice.
Type 'k' by pressing button 2 twice.
Type 'l' by pressing button 3 twice.
A total of 15 button presses are needed, so return 15.

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.

```cpp
class Solution {
public:
    int minimumKeypresses(string s) {
        int n=s.size();
        //get freq map
        unordered_map<char, int> freq;
        for(char c: s){ freq[c]++; }
        //sort by freq
        vector<pair<int,char>> v;
        for(auto [c,f]: freq){ v.push_back({f,c}); }
        sort(v.begin(), v.end(), greater<>());
        //assign typeX
        int res=0;
        int type1=9, type2=9, type3=9;//type3 not required
        for(auto [f,c]: v){
            if(type1>0){
                //can assign type1
                type1--;
                res+=f*1;
            }
            else if(type2>0){
                //can assign type2
                type2--;
                res+=f*2;
            }
            else{
                //can assign type3
                type3--;//not required
                res+=f*3;
            }
        }
        return res;
    }
};
/*
divide the 26 chars to 9 groups, each group at most has 3 chars, chars have seq
for each c in s, 
    res+= c's position in its group
return: min res
================================
len: 1~1e5
lowercase only: 26
================================
s -> freq<char,cnt>
for each one from high freq to low freq:
    assign as smaller type as possible
    mp[char,type?]
positions: type1*9, type2*9, type3*9

*/
```