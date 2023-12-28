# String Compression

#: 443
Difficult: Medium
Tags: 2pt, string
link: https://leetcode.com/problems/string-compression
Priority: High
Status: Dig More, HardToImplement, More Solution
Created time: July 17, 2023 4:25 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_75

Given an array of characters `chars`, compress it using the following algorithm:

Begin with an empty string `s`. For each group of **consecutive repeating characters** in `chars`:

- If the group's length is `1`, append the character to `s`.
- Otherwise, append the character followed by the group's length.

The compressed string `s` **should not be returned separately**, but instead, be stored **in the input character array `chars`**. Note that group lengths that are `10` or longer will be split into multiple characters in `chars`.

After you are done **modifying the input array,** return *the new length of the array*.

You must write an algorithm that uses only constant extra space.

**Example 1:**

```
Input: chars = ["a","a","b","b","c","c","c"]
Output: Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]
Explanation: The groups are "aa", "bb", and "ccc". This compresses to "a2b2c3".

```

**Example 2:**

```
Input: chars = ["a"]
Output: Return 1, and the first character of the input array should be: ["a"]
Explanation: The only group is "a", which remains uncompressed since it's a single character.

```

**Example 3:**

```
Input: chars = ["a","b","b","b","b","b","b","b","b","b","b","b","b"]
Output: Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].
Explanation: The groups are "a" and "bbbbbbbbbbbb". This compresses to "ab12".
```

**Constraints:**

- `1 <= chars.length <= 2000`
- `chars[i]` is a lowercase English letter, uppercase English letter, digit, or symbol.

Solution 1:

1pt,

```cpp
class Solution {
public:
    int compress(vector<char>& chars) {
        chars.push_back('|');
        vector<char> res;
        char last=chars[0];
        int cnt=0;
        for(auto c: chars){
            if(c==last) cnt++;
            else{
                res.push_back(last);
                if(cnt!=1){
                    for(auto c:to_string(cnt)) res.push_back(c);
                }
                last=c;
                cnt=1;
            }
        }
        chars=res;
        return chars.size();
    }
};
```

Solution 2:

2pt:

```cpp
class Solution {
public:
    int compress(vector<char>& chars) {
        int write=0;
        int read=0;
        int start=0;
        for(;read<chars.size();read++){
            //if same char block end
            if(read==chars.size()-1 || chars[read]!=chars[read+1]){
                chars[write++]=chars[start];
                if(start<read){
                    for(auto c:to_string(read-start+1)){
                        chars[write++]=c;
                    }
                }
                start=read+1;
            }
        }
        return write;
    }
};
```