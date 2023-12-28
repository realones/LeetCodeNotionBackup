# Sentence Screen Fitting

#: 418
Difficult: Medium
Tags: DP, simulation, string
link: https://leetcode.com/problems/sentence-screen-fitting/description/
Priority: High
Status: HardToImplement, todo
Created time: December 6, 2023 3:39 AM
Last edited time: December 6, 2023 3:49 AM
source: ticktokFreq

Given a `rows x cols` screen and a `sentence` represented as a list of strings, return *the number of times the given sentence can be fitted on the screen*.

The order of words in the sentence must remain unchanged, and a word cannot be split into two lines. A single space must separate two consecutive words in a line.

**Example 1:**

```
Input: sentence = ["hello","world"], rows = 2, cols = 8
Output: 1
Explanation:
hello---
world---
The character '-' signifies an empty space on the screen.

```

**Example 2:**

```
Input: sentence = ["a", "bcd", "e"], rows = 3, cols = 6
Output: 2
Explanation:
a-bcd-
e-a---
bcd-e-
The character '-' signifies an empty space on the screen.

```

**Example 3:**

```
Input: sentence = ["i","had","apple","pie"], rows = 4, cols = 5
Output: 1
Explanation:
i-had
apple
pie-i
had--
The character '-' signifies an empty space on the screen.

```

**Constraints:**

- `1 <= sentence.length <= 100`
- `1 <= sentence[i].length <= 10`
- `sentence[i]` consists of lowercase English letters.
- `1 <= rows, cols <= 2 * 104`

Solution: TLE

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        for(auto& w: sentence){
            if(w.size()>cols) return 0;
        }

        int n=sentence.size();
        vector<ii> dp(cols);
        for(int j=0;j<cols;j++){
            //dp[j] = getRowCntAndFinishingCol(j);
            int rowCnt=0;
            int curCol=j;
            bool firstWord=true;
            for(string& word: sentence){
                if(!firstWord){
                    curCol+=2;
                    if(curCol>cols-1){
                        rowCnt++;
                        curCol=0;
                    }
                }
                if(curCol+word.size()-1<cols) curCol+=word.size()-1;
                else{
                    rowCnt++;
                    curCol=word.size()-1;
                }
                firstWord=false;
            }
            dp[j]={rowCnt,curCol};
        }

        int curRow=0;
        int curCol=0;
        int cnt=0;
        bool firstSen=true;
        while(curRow<rows){
            if(!firstSen){
                curCol+=2;
                if(curCol>cols-1){
                    curRow++;
                    curCol=0;
                }
            }
            auto [numOfRows,finishingCol] = dp[curCol];
            curRow+=numOfRows;
            curCol=finishingCol;
            if(curCol>cols-1){
                curRow++;
                curCol=0;
            }
            cnt++;
            firstSen=false;
        }
        cnt--;
        return cnt;
    }
};

/*
sentence:
    len: 1~100
    each.len: 1~10
    val: lowerCase 26
rows,cols: 1~2e4
================================
simulation
maintain a counter for each time complete a sentense
try to input one by one words

reuse: dp:
dp[startPoint]: start from point, what is the rows to use, and what is the finish col
startPoint=>j: 0~cols.size()-1
dp[j]=numOfRows, finishingJ

calculate dp[0]~dp[cols.size()-1]:
    T: 10* 2e4

then simulate:
    T: 2e4
================================
*/
```

Solution improve from TLE, but still TLE:

```cpp
using ii=pair<int,int>;

class Solution {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        int n=sentence.size();
        for(auto& w: sentence){
            if(w.size()>cols) return 0;
        }

        vector<ii> dp(cols,{-1,-1});
        auto getRowCntAndFinishingCol = [&](int j)->ii{
            if(dp[j]!=ii{-1,-1}) return dp[j];
            int rowCnt=0;
            int curCol=j;
            bool firstWord=true;
            for(string& word: sentence){
                if(!firstWord){
                    curCol+=2;
                    if(curCol>cols-1){
                        rowCnt++;
                        curCol=0;
                    }
                }
                if(curCol+word.size()-1<cols) curCol+=word.size()-1;
                else{
                    rowCnt++;
                    curCol=word.size()-1;
                }
                firstWord=false;
            }
            dp[j]={rowCnt,curCol};
            return dp[j];
        };

        int curRow=0;
        int curCol=0;
        int cnt=0;
        bool firstSen=true;
        while(curRow<rows){
            if(!firstSen){
                curCol+=2;
                if(curCol>cols-1){
                    curRow++;
                    curCol=0;
                }
            }
            auto [numOfRows,finishingCol] = getRowCntAndFinishingCol(curCol);
            curRow+=numOfRows;
            curCol=finishingCol;
            if(curCol>cols-1){
                curRow++;
                curCol=0;
            }
            cnt++;
            firstSen=false;
        }
        cnt--;
        return cnt;
    }
};

/*
sentence:
    len: 1~100
    each.len: 1~10
    val: lowerCase 26
rows,cols: 1~2e4
================================
simulation
maintain a counter for each time complete a sentense
try to input one by one words

reuse: dp:
dp[startPoint]: start from point, what is the rows to use, and what is the finish col
startPoint=>j: 0~cols.size()-1
dp[j]=numOfRows, finishingJ

calculate dp[0]~dp[cols.size()-1]:
    T: 10* 2e4

then simulate:
    T: 2e4
================================
*/
```