# [lint] K Edit Distance

#: 623
Difficult: Hard
Tags: DFS, DP, StringMatchToMatrix, Trie
link: https://www.lintcode.com/problem/k-edit-distance/
Priority: High
Created time: September 15, 2022 8:24 PM
Last edited time: October 12, 2023 2:56 PM
source: jiuzhang, 算高DP下

Given a set of strings which just has lower case letters and a target string, output all the strings for each the edit distance with the target no greater than `k`.

You have the following 3 operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

Example:
**Example 1:**

```
Given words = `["abc", "abd", "abcd", "adc"]` and target = `"ac"`, k = `1`
Return `["abc", "adc"]`
Input:
["abc", "abd", "abcd", "adc"]
"ac"
1
Output:
["abc","adc"]

Explanation:
"abc" remove "b"
"adc" remove "d"

```

**Example 2:**

```
Input:
["acc","abcd","ade","abbcd"]
"abc"
2
Output:
["acc","abcd","ade","abbcd"]

Explanation:
"acc" turns "c" into "b"
"abcd" remove "d"
"ade" turns "d" into "b" turns "e" into "c"
"abbcd" gets rid of "b" and "d"

```

- edit distance + trie + dfs

将所有的原串放进一棵字典树，然后遍历这棵字典树(相当于遍历所有的前缀)，dp[i] 表示从Trie的root节点走到当前node节点，形成的Prefix和 target的前i个字符的最小编辑距离。最后通过dp[i]就可以得到和target的最小距离。

trie+dfs，

1. 保证到达k-distance后，就会剪枝 （如果不是这个，不用trie就行了应该，只是dp要用具体的prefixStr来表示了，以便复用同样的subquestion）
2. 同一路径的prefix共用一个dp row向下滚动；
3. 同一parent的children共用一个parent dp row，但是有不同的child dp row

```cpp
//implement trie
class Node{
public:
	bool end;
	vector<Node*> next;
	string str;
	Node(){
		end = false;
		str = "";
		next = vector<Node*>(26, NULL);
	}
};

class Trie{
public:
	Node* root;

	Trie(){
		root = new Node();
	}

	void insert(string word){
		Node* cur = root;
		for (auto c : word){
			if (!cur->next[c - 'a']){ cur->next[c - 'a'] = new Node(); }
			cur = cur->next[c - 'a'];
		}
		cur->end = true;
		cur->str = word;
	}
};

class Solution {
public:
	/*
	* @param words: a set of stirngs
	* @param target: a target string
	* @param k: An integer
	* @return: output all the strings that meet the requirements
	*/

	/*
	这题是将所有的原串放进一棵字典树，然后遍历这棵字典树(相当于遍历所有的前缀)，dp[i] 表示从Trie的root节点走到当前node节点，形成的Prefix和 target的前i个字符的最小编辑距离。最后通过dp[i]就可以得到和target的最小距离。

	edit distance + trie + dfs
	*/
	vector<string> kDistance(vector<string> &words, string &target, int k) {
		int m = words.size();
		int n = target.size();

		vector<string> res;
		Trie* trie = new Trie();
		Node* root = trie->root;
		for (auto word : words){ trie->insert(word); }
		//dp, just one row as the 2d dp matrix in "edit distance"
		vector<int> dp(n + 1, 0);
		//init
		for (int i = 0; i<n + 1; i++){ dp[i] = i; }

		//dfs
		helper(root, res, k, target, dp);

		return res;
	}

	void helper(Node* node, vector<string>& res, int k, string& target, vector<int>& dp) {
		int n = target.size();
		//find a word with distance <= k
		if (node->end && dp[n] <= k){
			res.push_back(node->str);
		}

		vector<int> next(n + 1, 0);//next row in edit distance
		//dfs
		for (int i = 0; i<26; i++){
			if (!node->next[i]) continue;

			next[0] = dp[0] + 1;//init the first colomn

			/*edit distance
			for (int i = 1; i < row; i++)
			for (int j = 1; j < col; j++){
			if (word1[i-1] == word2[j-1])
			f[i][j] = f[i-1][j-1];
			else
			f[i][j] = min(f[i-1][j-1], min(f[i-1][j], f[i][j-1])) + 1;
			}
			*/
			char sourceChar = i + 'a';
			for (int j = 1; j <= n; j++) {
				if (target[j - 1] == sourceChar) {
					next[j] = dp[j - 1];
				}
				else {
					next[j] = min(dp[j - 1], min(dp[j], next[j - 1])) + 1;
				}
			}
			helper(node->next[i], res, k, target, next);
		}

	}
};
```