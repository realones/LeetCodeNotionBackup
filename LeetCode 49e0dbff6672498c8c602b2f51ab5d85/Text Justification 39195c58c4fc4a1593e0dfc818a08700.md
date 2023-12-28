# Text Justification

#: 68
Difficult: Hard
Tags: string
link: https://leetcode.com/problems/text-justification/
Priority: Medium
Status: checkBetterSolution
Created time: September 15, 2022 11:22 PM
Last edited time: October 12, 2023 2:56 PM
source: LC_Top_Interview_150

Given an array of strings `words` and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly `maxWidth` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

**Note:**

- A word is defined as a character sequence consisting of non-space characters only.
- Each word's length is guaranteed to be greater than `0` and not exceed `maxWidth`.
- The input array `words` contains at least one word.

**Example 1:**

```
Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

**Example 2:**

```
Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
Note that the second line is also left-justified because it contains only one word.
```

**Example 3:**

```
Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

**Constraints:**

- `1 <= words.length <= 300`
- `1 <= words[i].length <= 20`
- `words[i]` consists of only English letters and symbols.
- `1 <= maxWidth <= 100`
- `words[i].length <= maxWidth`

```cpp
class Solution {
public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        int n=words.size(); if(n==0) {return {};}
        vector<string> res;
        for(int i=0;i<n;){
            vector<string> tmp;
            int sz=0;
            while(i<words.size() && sz+words[i].size()<=maxWidth){
                tmp.push_back(words[i]);
                sz+=words[i].size()+1;
                i++;
            }
            if(i!=n){
                res.push_back(arrangeLine(tmp,maxWidth));
            }
            else{
                res.push_back(arrangeLastLine(tmp,maxWidth));
            }
        }
        return res;
    }
    
    string arrangeLine(vector<string>& words, int maxWidth){
        string res="";
        int n=words.size();
        int gap=n-1;
        int totalWordsSz=0;
        for(auto w: words){
            totalWordsSz+=w.size();
        }
        int spaces=maxWidth-totalWordsSz;
        if(gap==0){
            res+=words.back()+string(spaces,' ');
        }
        else{
            int spaceBase=spaces/gap;
            int AdditionalOneSpaceGapSz=spaces%gap;
            for(int i=0;i<=n-2;i++){
               int spacesToAdd=spaceBase;
               if(i<AdditionalOneSpaceGapSz){
                   spacesToAdd+=1;
               }
               res+=words[i]+string(spacesToAdd,' ');
            }
            res+=words.back();
        }
        return res;
    }
    
    string arrangeLastLine(vector<string>& words, int maxWidth){
        string res="";
        int n=words.size();
        for(int i=0;i<=n-2;i++){
           res+=words[i]+" ";
        }
        res+=words.back();
        int spaces=maxWidth-res.size();
        res+=string(spaces,' ');
        return res;
    }
};
```