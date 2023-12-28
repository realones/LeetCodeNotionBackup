# Zigzag Conversion

#: 6
Difficult: Medium
Tags: string
link: https://leetcode.com/problems/zigzag-conversion/
Priority: Pass
Created time: May 31, 2023 4:07 AM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R

```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows);

```

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"

```

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I

```

**Example 3:**

```
Input: s = "A", numRows = 1
Output: "A"

```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consists of English letters (lower-case and upper-case), `','` and `'.'`.
- `1 <= numRows <= 1000`

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows==1) return s;
        
        bool down=1;//1:down, 0:up
        vector<vector<char>> m(numRows,vector<char>(s.size(),0));
        int i=0,j=0;
        for(auto c:s){
            if(i==0) down=1;//down
            m[i][j]=c;
            if(down) i++;
            else i--,j++;
            if(i==numRows-1) down=0;
        }
        string res;
        for(int i=0;i<m.size();i++){
            for(int j=0;j<m[0].size();j++){
                if(m[i][j]) res+=m[i][j];
            }
        }
        return res;
    }
};
```