# Evaluate Reverse Polish Notation

#: 150
Difficult: Medium
Tags: simulation, stack
link: https://leetcode.com/problems/evaluate-reverse-polish-notation/
Priority: Pass
Created time: June 4, 2023 1:37 PM
Last edited time: October 12, 2023 2:56 PM
source: HuaHua, LC_Top_Interview_150

You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return *an integer that represents the value of the expression*.

**Note** that:

- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a **32-bit** integer.

**Example 1:**

```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

```

**Example 2:**

```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

```

**Example 3:**

```
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

```

**Constraints:**

- `1 <= tokens.length <= 104`
- `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.

```cpp
class Solution {
public:
	int evalRPN(vector<string>& tokens) {
		if (tokens.empty()) return 0;
		stack<string> stk;
		for (auto s : tokens) {
			if (s!="+"&&s!="-"&&s!="*"&&s!="/") { stk.push(s); }
			else {
				int n2 = stoi(stk.top()); stk.pop();
				int n1 = stoi(stk.top()); stk.pop();
				int val;
				if (s == "+") { val = n1 + n2; }
				else if (s == "-") { val = n1 - n2; }
				else if (s == "*") { val = n1 * n2; }
				else if (s == "/") { val = n1 / n2; }
				stk.push(to_string(val));
			}
		}
		return stoi(stk.top());
	}
};
```