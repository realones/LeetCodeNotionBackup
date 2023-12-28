# Scramble String

#: 87
Difficult: Hard
Tags: DFS, DP, string
link: https://leetcode.com/problems/scramble-string/
Priority: Low
Status: More Solution
Created time: September 14, 2022 4:24 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, jiuzhang, 算高DP下

We can scramble a string s to get a string t using the following algorithm:

1. If the length of the string is 1, stop.
2. If the length of the string is > 1, do the following:
    - Split the string into two non-empty substrings at a random index, i.e., if the string is `s`, divide it to `x` and `y` where `s = x + y`.
    - **Randomly** decide to swap the two substrings or to keep them in the same order. i.e., after this step, `s` may become `s = x + y` or `s = y + x`.
    - Apply step 1 recursively on each of the two substrings `x` and `y`.

Given two strings `s1` and `s2` of **the same length**, return `true` if `s2` is a scrambled string of `s1`, otherwise, return `false`.

**Example 1:**

```
Input: s1 = "great", s2 = "rgeat"
Output: true
Explanation: One possible scenario applied on s1 is:
"great" --> "gr/eat" // divide at random index.
"gr/eat" --> "gr/eat" // random decision is not to swap the two substrings and keep them in order.
"gr/eat" --> "g/r / e/at" // apply the same algorithm recursively on both substrings. divide at random index each of them.
"g/r / e/at" --> "r/g / e/at" // random decision was to swap the first substring and to keep the second substring in the same order.
"r/g / e/at" --> "r/g / e/ a/t" // again apply the algorithm recursively, divide "at" to "a/t".
"r/g / e/ a/t" --> "r/g / e/ a/t" // random decision is to keep both substrings in the same order.
The algorithm stops now, and the result string is "rgeat" which is s2.
As one possible scenario led s1 to be scrambled to s2, we return true.

```

**Example 2:**

```
Input: s1 = "abcde", s2 = "caebd"
Output: false

```

**Example 3:**

```
Input: s1 = "a", s2 = "a"
Output: true

```

**Constraints:**

- `s1.length == s2.length`
- `1 <= s1.length <= 30`
- `s1` and `s2` consist of lowercase English letters.

Solution 1:

- 看 f[great][rgreat] 这个参考例子
- f[gr|eat][rgreat] =
    - f[gr][rg] && f[eat][eat]
    - f[gr][at] && f[eat][rgr]

```cpp
f(source[i,j], target[k,t]):
	if source[i...j] and target[k…t] can be scramble string
	notice that their lenghs are same

f(source[i,j], target[k,t]) =
	if exist “x”, that divide str in two parts,
		f(source[i,i+x], target[k,k+x]) && f(source[i+x+1,j], target[k+x+1,t]) //if not reverse
		||
		f(source[i,i+x], target[t-x,t]) && f(source[i+x+1,j], target[t-x-1,t]) //if reverse

result:
	f(whole source str, whole target str)

init:
	check if two single chars are same
```

Solution 2 (from jiuzhang)

- State:
    - dp[x][y][k] 表示是从s1串x开始，s2串y开始，他们后面k个字符组成的substr是Scramble String
- Function:
    - 对于所有i属于{1,k}
    - s11 = s1.substring(0, i); s12 = s1.substring(i, s1.length());
    - s21 = s2.substring(0, i); s22 = s2.substring(i, s2.length());
    - s23 = s2.substring(0, s2.length() - i); s24 = s2.substring(s2.length() - i, s2.length());
    - for i = x -> x+k
        - dp[x][y][k] = (dp[x][y][i] && dp[x+i][y+i][k-i]) || dp[x][y+k-i][i] && dp[x+i][y][k-i])
- Intialize:
    - dp[i][j][1] = s1[i]==s[j].
- Answer:
    - dp[0][0][len]

```cpp
/*
State:
dp[x][y][k] 表示是从s1串x开始，s2串y开始，他们后面k个字符组成的substr是Scramble String

Function:
对于所有i属于{1,k}
s11 = s1.substring(0, i); s12 = s1.substring(i, s1.length());
s21 = s2.substring(0, i); s22 = s2.substring(i, s2.length());
s23 = s2.substring(0, s2.length() - i); s24 = s2.substring(s2.length() - i, s2.length());
for i = x -> x+k
dp[x][y][k] = (dp[x][y][i] && dp[x+i][y+i][k-i]) || dp[x][y+k-i][i] && dp[x+i][y][k-i])

Intialize:
dp[i][j][1] = s1[i]==s[j].

Answer:
dp[0][0][len]
*/
class Solution {
public:
	unordered_map<string,bool> dp;

	bool isScramble(string s1, string s2) {
		if (s1.size() != s2.size()) return false;
		if (s1 == s2) return true;
		int len = s1.size();

		//check dp
		string key = s1+s2;
		if (dp.count(key)){ return dp[key]; }

		//check if they are anagrams
		unordered_map<char,int> mp;
		for (int i = 0; i<len; i++){
			mp[s1[i]]++;
			mp[s2[i]]--;
		}
		for (auto p: mp){
			if (p.second!=0) return false;
		}
		//dfs
		bool res=false;
		for (int sz = 1; sz <= len - 1; sz++){
			if (isScramble(s1.substr(0, sz), s2.substr(0, sz)) && isScramble(s1.substr(sz), s2.substr(sz)))
			{res = true; break;}
			if (isScramble(s1.substr(0, sz), s2.substr(len - sz)) && isScramble(s1.substr(sz), s2.substr(0, len - sz)))
			{res = true; break;}
		}
		dp[key] = res;
		return dp[key];
	}
};
```