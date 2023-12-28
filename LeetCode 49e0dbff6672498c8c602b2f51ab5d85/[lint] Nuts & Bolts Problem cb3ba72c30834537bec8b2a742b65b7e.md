# [lint] Nuts & Bolts Problem

#: 399
Difficult: Medium
Tags: Partition_with_pivotIdx, quickSort
link: https://www.lintcode.com/problem/nuts-bolts-problem/
Priority: High
Created time: September 25, 2022 4:28 PM
Last edited time: November 1, 2023 8:19 PM
practicedTimes: 1
source: jiuzhang, 算高FollowUp

Given a set of *n* nuts of different sizes and *n* bolts of different sizes. There is a one-one mapping between nuts and bolts.

Comparison of a nut to another nut or a bolt to another bolt is **not allowed**. It means nut can only be compared with bolt and bolt can only be compared with nut to see which one is bigger/smaller.  We will give you a compare function to compare nut with bolt.

Using the function we give you, you are supposed to sort nuts or bolts, so that they can map in order.

Example:
Given nuts = `['ab','bc','dd','gg']`, bolts = `['AB','GG', 'DD', 'BC']`.

Your code should find the matching of bolts and nuts.

According to the function, one of the possible return:

nuts = `['ab','bc','dd','gg']`, bolts = `['AB','BC','DD','GG']`.

If we give you another compare function, the possible return is the following:

nuts = `['ab','bc','dd','gg']`, bolts = `['BC','AA','DD','GG']`.

So you must use the compare function that we give to do the sorting.

The order of the nuts or bolts does not matter. You just need to find the matching bolt for each nut.

```cpp
/**
 * class Comparator {
 *     public:
 *      int cmp(string a, string b);
 * };
 * You can use compare.cmp(a, b) to compare nuts "a" and bolts "b",
 * if "a" is bigger than "b", it will return 1, else if they are equal,
 * it will return 0, else if "a" is smaller than "b", it will return -1.
 * When "a" is not a nut or "b" is not a bolt, it will return 2, which is not valid.
*/
class Solution {
public:
	/**
	* @param nuts: a vector of integers
	* @param bolts: a vector of integers
	* @param compare: a instance of Comparator
	* @return: nothing
	*/

	//quick sort - partition
	void sortNutsAndBolts(vector<string> &nuts, vector<string> &bolts, Comparator compare) {
		int m = nuts.size(); if (m == 0) return;
		int n = bolts.size(); if (n != m) return;

		quickSort(nuts, bolts, 0, n - 1, compare);
	}

	void quickSort(vector<string> &nuts, vector<string> &bolts, int start, int end, Comparator& compare){
        //need validation because partition do start+1 till end
		if (start >= end) return;

        //partition bolts with nuts[start], find the parition idx that bolts[partIdx] match nuts[start]
        int partIdx = partition(bolts, nuts[start], start, end, compare);
        partition(nuts, bolts[partIdx], start, end, compare);

        quickSort(nuts, bolts, start, partIdx-1);
        quickSort(nuts, bolts, partIdx+1, end);
    }

	int partition(vector<string> &arr, string pivot, int start, int end, Comparator& compare){

        //find pivot, swap with start
        for(int i=start; i<=end; i++){
            if(compare.cmp(arr[i], pivot) == 0 ||
               compare.cmp(pivot, arr[i]) == 0){swap(arr[start],arr[i]);break;}
        }

        //do partition from start+1 to end
		int l = start+1, r = end;
		while (l <= r){
			while (l <= r && (compare.cmp(pivot, arr[l]) == 1 || compare.cmp(arr[l], pivot) == -1)) l++;
			while (l <= r && (compare.cmp(pivot, arr[r]) == -1 || compare.cmp(arr[r], pivot) == 1)) r--;
			if (l <= r){
				swap(arr[l], arr[r]);
				l++, r--;
			}
		}

        //put the pivot between r and l
        swap(arr[start],arr[r]);
        return r;
	}
};
```