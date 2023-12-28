# [lint] Continuous Subarray Sum II

#: 403
Difficult: Medium
Tags: Contribution>0, Prefix
link: https://www.lintcode.com/problem/continuous-subarray-sum-ii
Priority: Medium
Created time: September 18, 2022 6:55 PM
Last edited time: November 1, 2023 8:13 PM
practicedTimes: 1
source: jiuzhang, 算高FollowUp

Given an circular integer array (the next element of the last element is the first element), find a continuous subarray in it, where the sum of numbers is the biggest. Your code should return the index of the first number and the index of the last number.

If duplicate answers exist, return any of them.

Example:
**Example 1:**

```
Input: [3, 1, -100, -3, 4]
Output: [4, 1]

```

**Example 2:**

```
Input: [1,-1]
Output: [0, 0]

```

Challenge
O(*n*) time

“但是为了防止出现最小数组包含头尾的情况（即end＋1或start－1越界）”

好像没有必要，反正都是要offset = (offset%size + size)%size;

```cpp
class Solution {
public:
	/*
	* @param A: An integer array
	* @return: A list of integers includes the index of the first number and the index of the last number
	*/
	/*
	最大数组的范围只有两种可能：1. [ i ~ j ]，2. [ i ~ N-1] + [ 0 ~ j ].
	所以，你只要分别找到两种情况的最大者，取这两个最大者中较大的即可。

	1和Continuous Subarray Sum I相同，就不多说了。

	2等价于找一个范围[j+1 ~i-1]，    使得这个范围内的数组和最小。
	这又等价于将原数组取负号，然后在这个负数组中找最大和的[j+1 ~ i+1]范围即可。
	*/

	//取反
	vector<int> continuousSubarraySumII(vector<int> &A) {
		int n = A.size(); if (n == 0) return vector<int>();
		//no cicle
		pair<int, vector<int>> res1 = continuousSubarraySum(A);

		//cicle
		int allSum = 0;//sum of A
		for (auto a : A){ allSum += a; }

		vector<int> reA;//reA=-A
		for (auto a : A){ reA.push_back(-a); }

		/*
		1 要考虑一种情况为整个数组全部为负数，这样需要减去的是整个数组，因此返回1中的结果

		2 根据2中求的最小数组，最大数组的index因该为end＋1到start－1，
		  但是为了防止出现最小数组包含头尾的情况（即end＋1或start－1越界），
		  index的表示方法为(end + 1) % A.length和(start - 1 + A.length) % A.length。
		*/

		pair<int, vector<int>> res2 = continuousSubarraySum(reA);
		if (allSum == -res2.first) return res1.second;
		res2.first = allSum - (-res2.first);
		reverse(res2.second.begin(), res2.second.end());
		res2.second[0] = (res2.second[0] + 1) % n;
		res2.second[1] = (res2.second[1] - 1 + n) % n;

		if (res1.first>res2.first) return res1.second;
		else return res2.second;
	}

	pair<int, vector<int>> continuousSubarraySum(vector<int> &A) {
		int n = A.size(); if (n == 0) return pair<int, vector<int>>();
		int l = 0, r = 0;
		int left, right;
		int tmpSum = 0, sum = INT_MIN;
		for (int i = 0; i<n; i++){
			tmpSum += A[i];
			if (tmpSum>sum){
				sum = tmpSum;
				r = i;

				left = l, right = r;
			}
			if (tmpSum<0){
				l = i + 1;
				tmpSum = 0;
			}
		}

		return pair<int, vector<int>>{sum, { left, right }};
	}

};
```