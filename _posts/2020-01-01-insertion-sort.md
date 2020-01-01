---
layout: post
title: "Insertion Sort"
author: "DAEUN"
---

There are many sorting algorithms such as insertion, select, bubble, heap sort, and so on. Today, we'll going to talk about insertion sort.
<br>
### Insertion Sort
Let's assume there is an array named _A_ where the elements are 10, 3, 5, 1, 8. To sort this array using insertion sort algorithm, we start sorting from the first element, 10. Since there is only one element we sorted so far, let's go to the next element, 3. Comparing 10 and 3, 10 should move to the next position, A[1], and 3 should move where 10 was at, A[0], because 10 is bigger than 3. Now, the order of the array _A_ will look like 3, 10, 5, 1, 8. Next, we need to start sorting again starting from A[2]. Comparing 10 and 5, 10 is bigger than 5 so that 10 moves to A[2]. Again, comparing 3 and 5, 3 is smaller than 5 so that 3 doesn't move and 5 is put into A[1]. You can repeat this steps through the end of the array and finally sort the array. The total steps are shown below.
<br><br>
![insertion sort](/assets/images/insertion_sort.PNG)
<br><br>
Insertion sort can be implemented as the code below.
<br>
```c++
void insertion_sort(int array[], int n) {
	int i, j, temp = 0;
	for(; i < n; i++) {
		temp = array[i];
		for(j = i-1; array[j] > temp; j--) {		//comparing two numbers in array
			array[j+1] = array[j]			//move bigger number to the right
		}
		array[j+1] = temp;			//put array[i] into the empty space
	}
}
```
<br><br>
The best case of this algorithm it when all the elemtents in the array are already sorted. Then, it only compares numbers _n_ times, A[0] to A[n-1], so that the complexity of the best case is **&Theta;(n)**. On the other hand, the worst case is when all the elements are sorted in descending order. In this case, this algorithm compares numbers _nx(n-1)_ times, all the elements for two for-loops, so that the complexity of the worst case is **O(n<sup>2</sup>)**.