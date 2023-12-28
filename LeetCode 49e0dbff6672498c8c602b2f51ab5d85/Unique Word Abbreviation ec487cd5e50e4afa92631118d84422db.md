# Unique Word Abbreviation

#: 288
Difficult: Medium
Tags: Hash
link: https://leetcode.com/problems/unique-word-abbreviation/
Priority: Pass
Created time: October 16, 2022 6:34 PM
Last edited time: October 12, 2023 2:56 PM
source: googleFreq

The **abbreviation** of a word is a concatenation of its first letter, the number of characters between the first and last letter, and its last letter. If a word has only two characters, then it is an **abbreviation** of itself.

For example:

- `dog --> d1g` because there is one letter between the first letter `'d'` and the last letter `'g'`.
- `internationalization --> i18n` because there are 18 letters between the first letter `'i'` and the last letter `'n'`.
- `it --> it` because any word with only two characters is an **abbreviation** of itself.

Implement the `ValidWordAbbr` class:

- `ValidWordAbbr(String[] dictionary)` Initializes the object with a `dictionary` of words.
- `boolean isUnique(string word)` Returns `true` if **either** of the following conditions are met (otherwise returns `false`):
    - There is no word in `dictionary` whose **abbreviation** is equal to `word`'s **abbreviation**.
    - For any word in `dictionary` whose **abbreviation** is equal to `word`'s **abbreviation**, that word and `word` are **the same**.

**Example 1:**

```
Input
["ValidWordAbbr", "isUnique", "isUnique", "isUnique", "isUnique", "isUnique"]
[[["deer", "door", "cake", "card"]], ["dear"], ["cart"], ["cane"], ["make"], ["cake"]]
Output
[null, false, true, false, true, true]

Explanation
ValidWordAbbr validWordAbbr = new ValidWordAbbr(["deer", "door", "cake", "card"]);
validWordAbbr.isUnique("dear"); // return false, dictionary word "deer" and word "dear" have the same abbreviation "d2r" but are not the same.
validWordAbbr.isUnique("cart"); // return true, no words in the dictionary have the abbreviation "c2t".
validWordAbbr.isUnique("cane"); // return false, dictionary word "cake" and word "cane" have the same abbreviation  "c2e" but are not the same.
validWordAbbr.isUnique("make"); // return true, no words in the dictionary have the abbreviation "m2e".
validWordAbbr.isUnique("cake"); // return true, because "cake" is already in the dictionary and no other word in the dictionary has "c2e" abbreviation.

```

**Constraints:**

- `1 <= dictionary.length <= 3 * 104`
- `1 <= dictionary[i].length <= 20`
- `dictionary[i]` consists of lowercase English letters.
- `1 <= word.length <= 20`
- `word` consists of lowercase English letters.
- At most `5000` calls will be made to `isUnique`.

```cpp
class ValidWordAbbr {
public:
    unordered_map<string, unordered_set<string>> mp;
    
    ValidWordAbbr(vector<string>& dictionary) {
        for(auto s: dictionary){
            mp[getAbbr(s)].insert(s);
        }
    }
    
    bool isUnique(string word) {
        string wordAbbr=getAbbr(word);
        if(!mp.count(wordAbbr)) return true;
        return mp[wordAbbr].size()==1 && mp[wordAbbr].count(word);
				//not realize mp[wordAbbr].size()==1, if so we can use mp<str, str> instead
    }
    
    string getAbbr(string s){
        if(s.size()<=2) return s;
        return s.front()+to_string(s.size()-2)+s.back();
    }
};

/**
 * Your ValidWordAbbr object will be instantiated and called as such:
 * ValidWordAbbr* obj = new ValidWordAbbr(dictionary);
 * bool param_1 = obj->isUnique(word);
 */
```