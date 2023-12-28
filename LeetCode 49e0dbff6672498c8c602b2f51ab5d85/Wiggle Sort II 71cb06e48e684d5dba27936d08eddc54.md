# Wiggle Sort II

#: 324
Difficult: Medium
Tags: QuickSelect
link: https://leetcode.com/problems/wiggle-sort-ii/
Priority: Medium
Status: More Solution
Created time: September 23, 2022 5:25 PM
Last edited time: November 1, 2023 8:20 PM
practicedTimes: 1
source: jiuzhang, 算高FollowUp

Given an integer array `nums`, reorder it such that `nums[0] < nums[1] > nums[2] < nums[3]...`.

You may assume the input array always has a valid answer.

**Example 1:**

```
Input: nums = [1,5,1,1,6,4]
Output: [1,6,1,5,1,4]
Explanation: [1,4,1,5,1,6] is also accepted.

```

**Example 2:**

```
Input: nums = [1,3,2,2,3,1]
Output: [2,3,1,3,1,2]

```

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `0 <= nums[i] <= 5000`
- It is guaranteed that there will be an answer for the given input `nums`.

**Follow Up:**

Can you do it in

```
O(n)
```

time and/or

**in-place**

with

```
O(1)
```

extra space?

```cpp
/*
方法二

用快排的思想查找中位数，然后再合并两边。用方法一下办部分
最坏复杂度O(n^2)，平均复杂度O(n)
*/
class Solution {
public:
	void wiggleSort(vector<int>& nums) {
		int n = nums.size();
		int median = getKthElement(nums, 0, n - 1, (n+1)/2);//get median
		//build tmp, roughly sorted
		vector<int> tmp(n, median);
		int l = 0, r = n - 1;
		for (int i = 0; i<n; i++){
			if (nums[i]<median){ tmp[l++] = nums[i]; }
			else if (nums[i]>median){ tmp[r--] = nums[i]; }
		}

		//same as solution 1, reversely insert
		l = (n + 1) / 2 - 1, r = n - 1;//保证左侧size=右侧（+1）
		bool even = true;
		for (int i = 0; i<n; i++, even = !even){
			nums[i] = even ? tmp[l--] : tmp[r--];
		}
	}

	//partion
	int getKthElement(vector<int>& nums, int start, int end, int k){
		if (start == end) return nums[start];

		int pivot = nums[start + (end - start) / 2];
		int l = start, r = end;
		while (l <= r){
			while (l <= r && nums[l]<pivot) l++;
			while (l <= r && nums[r]>pivot) r--;
			if (l <= r){
				swap(nums[l], nums[r]);
				l++, r--;
			}
		}
		if (start + k - 1 <= r){ return getKthElement(nums, start, r, k); }
		else if (start + k - 1 >= l){ return getKthElement(nums, l, end, k - (l - start)); }
		else{ return nums[r + 1]; }
	}
};

/*
！！！！！以下为另一种解法！！！！！
这道题其实就是要找一个中位数，比中位数小的插入到偶数位上，比中位数大的插入到奇数位上。
用quick select寻找中位数（即能平分nums中所有元素的元素的值）

将ans的值全部设为中位数，遍历数组nums，将遇到的数和中位数比较，间隔地插入ans中
若原数组长度为偶数，则为小->大->小。。。->大的形式，小的数从后往前(len - 2)寻找位置插入，大的数从前往后(1)寻找位置插入
若原数组长度为奇数，则为大->小->大。。。->小的形式，小的数从前往后(0)寻找位置插入，大的数从后往前(len - 2)寻找位置插入
*/
//================================== 'New Comment                ' =====================================
// /*
// 方法一
// 
// 排序，然后两边分别取，复杂度O(nlogn)
// 注意排完序之后应该倒着来。比如[4,5,5,6]这个数据。这样可以让中间相等的数据分隔开
// */
// class Solution {
// public:
//     void wiggleSort(vector<int>& nums) {
//         sort(nums.begin(),nums.end());
//         int n=nums.size();
//         vector<int> tmp(n,0);
//         int l=(n+1)/2 - 1, r=n-1;
//         bool evenI=true;
//         for(int i=0;i<n;i++,evenI=!evenI){
//             tmp[i]=evenI? nums[l--]: nums[r--];
//         }
//         nums=tmp;
//     }
// };
```