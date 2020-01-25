---
layout: post
title: "Merge Sort"
author: "DAEUN"
---

As the merge sort algorithm is shown below, it constantly divide the array into halves and sort them in correct order.
<br>
![merge sort](/assets/images/merge_sort.jpg)
<br><br>
```c++
void merge(vector<int>& A, left, middle, right) {
	int i, j, k = 0;

	vector<int> L, R;	//auxiliary spaces
	L.resize(middle - left + 1);
	R.resize(right - middle);

	for(; i < middle - left + 1; i++)
		L.at(i) = A.at(left + i);
	for(; j < right - middle; j++)
		R.at(j) = A.at(middle + 1 + j);

	i, j = 0;
	k = left;
	while(i < L.size() && j < R.size()) {
		if(L.at(i) <= R.at(j)) {
			A.at(k) = L.at(i);
			i++;
		} else {
			A.at(k) = R.at(j);
			j++;
		}
		k++;
	}

	if(i < L.size()) {					//put the rest of the elements if there are any
		for(; i < L.size(); i++) {
			A.at(k) = L.at(i);
			k++;
		}
	} else if(j < R.size()) {
		for(; j <R.size(); j++) {
			A.at(k) = R.at(j);
			k++;
		}
	}
}

void mergeSort(vector<int>& A, left, right) {
	if(left < right) {
		int middle = (left + right)/2;

		mergeSort(A, left, middle);
		mergeSort(A, middle+1, right);

		merge(A, left, middle, right);
	}
}
```
<br><br>
This sort algorithm takes T(n) = 2T(n/2) + &Theta;(n) time, which is **O(nlgn)**. This is better complexity compare to one of insertion sort(O(n<sup>2</sup>)), but it needs O(n) auxiliary space for temporary arrays L and R.