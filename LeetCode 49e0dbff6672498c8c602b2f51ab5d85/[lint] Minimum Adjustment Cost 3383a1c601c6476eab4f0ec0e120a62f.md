# [lint] Minimum Adjustment Cost

#: 91
Difficult: Medium
Tags: DP
link: https://www.lintcode.com/problem/minimum-adjustment-cost/
Priority: Low
Created time: September 17, 2022 12:21 AM
Last edited time: October 12, 2023 2:56 PM
source: jiuzhang, 算高DP下

Given an integer array, adjust each integers so that the difference of every adjacent integers are not greater than a given number target.

If the array before adjustment is **A**, the array after adjustment is **B**, you should minimize the sum of `|A[i]-B[i]|`

element range: [0,100]

Example:

```
Example 1:
	Input:  [1,4,2,3], target=1
	Output:  2

Example 2:
	Input:  [3,5,4,7], target=2
	Output:  1

```

Matrix to store ALL possible sub results

这里应该f(n,101)也可以

- State:
• f[i][v] 前i个数，第i个数调整为v，满足相邻两数<=target，所需要的最小代价
- Function:
• f[i][v] = min(f[i-1][v’] + |A[i]-v|, |v-v’| <= target)
- Answer:
• f[n][a[n]-target~a[n]+target] (好像不对，应该是min(f[n][?]))
- O(n * A * T)

```cpp
class Solution {
public:
	/**
	* @param A: An integer array.
	* @param target: An integer.
	*/

	/*
	// backpack DP
	f(i,j): 前i个数，第i个数调整为j，满足相邻两数<=target，所需要的最小代价

	f(i,j) = min(f[i-1][j’] + |A[i-1]-j|), for all j' that |j-j’| <= target
	f(i,j) =
	1. A[i-1]变成j的cost
	+
	2. min f(i,j') [|j-j’| <= target], 上一个数字A[i-2]变成j'的cost，满足相邻两数<=target

	init: first row all 0, other row all INT_MAX

	return: min in last row

	time: O(n*100*target)
	*/

	int MinAdjustmentCost(vector<int> A, int target) {
		int n = A.size();
		//init all INT_MAX, first row 0
		vector<vector<int>> f(n+1,vector<int>(100+1,INT_MAX));
		for (int j = 0; j <= 100; j++) f[0][j] = 0;
		//process
		for (int i = 1; i < n+1; i++){//start from 2nd row
			for (int j = 0; j < 101; j++){
				//for each element
				int curVal = A[i-1];
				int cost1 = abs(curVal - j);//1. A[i-1]变成j的cost

				//2. min f(i,j') [|j-j’| <= target], 上一个数字A[i-2]变成j'的cost，满足相邻两数<=target
				int l = max(0, j - target);
				int r = min(100, j + target);
				int cost2 = INT_MAX;
				for (int k = l; k <= r; k++){
					cost2 = min(cost2,f[i-1][k]);
					//f[i][j] = min(f[i][j], f[i - 1][k] + cost1);
				}
				f[i][j] = cost1 + cost2;
			}
		}
		//result is in the last row
		int res = INT_MAX;
		for (int i = 0; i < 101; i++){
			res = min(res, f[n][i]);
		}
		return res;
	}
};
```