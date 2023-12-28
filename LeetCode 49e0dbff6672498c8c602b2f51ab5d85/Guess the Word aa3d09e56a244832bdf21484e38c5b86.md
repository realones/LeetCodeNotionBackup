# Guess the Word

#: 843
Difficult: Hard
Tags: Game, miniMax
link: https://leetcode.com/problems/guess-the-word/
Priority: Medium
Status: Dig More, No Idea At First, todo
Created time: October 3, 2022 11:09 PM
Last edited time: October 18, 2023 3:50 AM
practicedTimes: InProgress
source: googleFreq

You are given an array of unique strings `words` where `words[i]` is six letters long. One word of `words` was chosen as a secret word.

You are also given the helper object `Master`. You may call `Master.guess(word)` where `word` is a six-letter-long string, and it must be from `words`. `Master.guess(word)` returns:

- -`1` if `word` is not from `words`, or
- an integer representing the number of exact matches (value and position) of your guess to the secret word.

There is a parameter `allowedGuesses` for each test case where `allowedGuesses` is the maximum number of times you can call `Master.guess(word)`.

For each test case, you should call `Master.guess` with the secret word without exceeding the maximum number of allowed guesses. You will get:

- **`"Either you took too many guesses, or you did not find the secret word."`** if you called `Master.guess` more than `allowedGuesses` times or if you did not call `Master.guess` with the secret word, or
- **`"You guessed the secret word correctly."`** if you called `Master.guess` with the secret word with the number of calls to `Master.guess` less than or equal to `allowedGuesses`.

The test cases are generated such that you can guess the secret word with a reasonable strategy (other than using the bruteforce method).

**Example 1:**

```
Input: secret = "acckzz", words = ["acckzz","ccbazz","eiowzz","abcczz"], allowedGuesses = 10
Output: You guessed the secret word correctly.
Explanation:
master.guess("aaaaaa") returns -1, because "aaaaaa" is not in wordlist.
master.guess("acckzz") returns 6, because "acckzz" is secret and has all 6 matches.
master.guess("ccbazz") returns 3, because "ccbazz" has 3 matches.
master.guess("eiowzz") returns 2, because "eiowzz" has 2 matches.
master.guess("abcczz") returns 4, because "abcczz" has 4 matches.
We made 5 calls to master.guess, and one of them was the secret, so we pass the test case.

```

**Example 2:**

```
Input: secret = "hamada", words = ["hamada","khaled"], allowedGuesses = 10
Output: You guessed the secret word correctly.
Explanation: Since there are two words, you can guess both.

```

**Constraints:**

- `1 <= words.length <= 100`
- `words[i].length == 6`
- `words[i]` consist of lowercase English letters.
- All the strings of `wordlist` are **unique**.
- `secret` exists in `words`.
- `10 <= allowedGuesses <= 30`

```cpp
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Master {
 *   public:
 *     int guess(string word);
 * };
 */

//minimax
// narrow the candidates after each time we call master.guess()
class Solution {
public:
    void findSecretWord(vector<string>& words, Master& master) {
        unordered_set<string> st(words.begin(), words.end());
        while(true){
            string guess=minimaxLoss(st);
            int matchCnt=master.guess(guess);
            
            if(matchCnt==guess.size()){
                return;
            }
                
            st.erase(guess);
            for (auto it = st.begin(); it != st.end();) {
                if (getMatchCnt(guess, *it) != matchCnt) {
                    it = st.erase(it);
                } else {
                    ++it;
                }
            }
        }
    }
    
    /*
    In order to maximize the number of words we elimiinate at each guess
    we choose a guess that partitions the potential candidate set roughly equally by all possible distances.
    That is, if we choose a guess that's roughly equally at distance 0 from 1/6 of all words, distance 1 from 1/6 of all words, etc., 
    we know that whatever distance the secret happens to be from the guess, we can eliminate a substantial number of words. 
    This is where we use a MinMax heuristic. 
    That property guarantees that the selected word partitions the candidate set well by distance and that it has the potential to eliminate the maximum number of elements.
    */
    string minimaxLoss(unordered_set<string>& st){
        string res;
        int miniMax=INT_MAX;
        for(auto& w: st){
            int curMaxLoss=maxLoss(w,st);
            if(curMaxLoss<miniMax){
                miniMax=curMaxLoss;
                res=w;
            }
        }
        return res;
    }
    
    //how bad s parition words
    //one matchCnt has too many words means partition bad
    int maxLoss(string s, unordered_set<string>& st){
        vector<int> matchCnt_wordCnt(s.size()+1,0);
        for(auto& word: st){
            matchCnt_wordCnt[getMatchCnt(word,s)]++;
        }
        return *max_element(matchCnt_wordCnt.begin(),matchCnt_wordCnt.end());
    }
    
    int getMatchCnt(string s1, string s2){
        int res=0;
        for(int i=0; i<s1.size(); i++){
            if(s1[i]==s2[i]){
                res++;
            }
        }
        return res;
    }
};
```