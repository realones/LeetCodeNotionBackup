# [lint] Sort Letters by Case

#: 49
Difficult: Medium
Tags: Partition
link: https://www.lintcode.com/problem/49/
Priority: Pass
Created time: August 9, 2022 11:18 AM
Last edited time: October 19, 2023 9:27 PM
practicedTimes: 1
source: jiuzhang, 算初TwoPointers

**Description**

Given a string `chars` which contains only letters. Sort it by lower case first and upper case second.In different languages, `chars` will be given in different ways. For example, the string `"abc"` will be given in following ways:

- Java: char[] chars = {'a', 'b', 'c'};
- Python：chars = ['a', 'b', 'c']
- C++：string chars = "abc";

You need to implement the in-place algorithm to solve this problem without returning any values, and we will determine if your result is correct based on the sorted chars.

# 

It's *NOT* necessary to keep the original order of lower-case letters and upper case letters.

**Example**

**Example 1:**

Input:

```
chars = "abAcD"

```

Output:

```
"acbAD"

```

Explanation:

You can also return "abcAD" or "cbaAD" or other correct answers.**Example 2:**

Input:

```
chars = "ABC"

```

Output:

```
"ABC"

```

Explanation:

You can also return "CBA" or "BCA" or other correct answers.

**Challenge**

Do it in one-pass and in-place.

```cpp
class Solution {
public:
    /*
     * @param chars: The letter array you should sort by Case
     * @return: nothing
     */
    void sortLetters(string &chars) {
        int l=0,r=chars.size()-1;
        while(l<=r){
            while(l<=r && islower(chars[l])){l++;}
            while(l<=r && isupper(chars[r])){r--;}
            if(l<=r){
                swap(chars[l],chars[r]);
                l++,r--;
            }
        }
        return;
    }
};
```