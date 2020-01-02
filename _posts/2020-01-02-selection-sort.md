---
layout: post
title: "Selection Sort"
author: "DAEUN"
---

Let's assume there is an array named _A_ where the elements are 10, 3, 5, 1, 8. To sort this array in increasing order, I'll find maximum number and swith it with the last element of the array. For instance, I check all elements in the array and found 10 is the maximum number. I switched this number with the last element, 8, so that the order of the elements will be 8, 3, 5, 1, 10. Since I already found the maximum number, I'll find the next maximum number checking _n-1_ elements(except for the last element that is already sorted).
<br><br>
![selection sort](/assets/images/selection_sort.png)
<br><br>
Selection sort can be implemented as the code below.
<br>
```c++
int selection_sort(vector<int>& array) {
	int max = 0;
	int temp;
	for(int i = array.size(); i > 0; i--) {
		for(int j = 0; j < i; j++) {
			if(array[max] < array[j])		//finds the maximum number
				max = j;			//saves index of the current maximum number
		}
		temp = array[i-1];			//switch the maximum number and the last element that is not sorted yet
		array[i-1] = array[max];
		array[max] = temp;
	}
}
```
<br><br>
The complexity of selection sort is **O(n<sup>2</sup>)**. This is because the outer for-loop iterates the number of the array, n times and the inner for-loop iterates (n-1) times. Therefore, n \* (n-1) times is O(n<sup>2</sup>) in asymptotic notation.