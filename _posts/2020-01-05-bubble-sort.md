---
layout: post
title: "Bubble Sort"
author: "DAEUN"
---

Let's assume there is an array named _A_ where the elements are 10, 3, 5, 1, 8. If we sort this array using bubble sort algorithm, we compare elements side by side and exchange numbers if the former element is bigger than the latter number. To give you an example, we first compare A[0] and A[1], which are 10 and 3. Since 10 is bigger than 3, we switch these numbers. Then the array A will be sorted like 3, 10, 5, 1, and 8. Next, we compare A[1] and A[2], which are 10 and 5. We exchange these numbers because A[1] is bigger than A[2]. We repeat this process like the image below until we get to the end of the array, which means we get one maximum number at A[4]. Then we do the same process starting from A[0] again, and get the next maximum numbers at A[3], A[2], A[1], and A[0].
<br><br>
![bubble sort](/assets/images/bubble_sort.png)
<br><br>
Bubble sort can be implemented as the code below.
<br>
```c++
int bubble_sort(vector<int>& array) {
	int temp;
	for (int i = array.size() - 1; i > 0; i--) {
		for (int j = 0; j < i; j++) {
			if(array[j] > array[j+1]) {		//switch elements when the former element is bigger than the latter element in the array.
				temp = array[j+1];
				array[j+1] = array[j];
				array[j] = temp;
			}
		}
	}
	return 0;
}
```
<br><br>
This code's complexity is (n-1) + (n-2) + ... + 2 + 1 = n \* n/2 = **O(n<sup>2</sup>)**. Since this complexity is same as insertion sort and selection sort that we've seen in the previous posts, this is not a better algorithm. Also, this compiler still iterates the for loops even the array is already sorted. Therefore, we can add flag in this code so that the compiler doesn't need to do unnecessary works like the code below.
<br>
```c++
int bubble_sort_flag(vector<int>& array) {
	int temp;
	bool isSwitched;		//flag
	for (int i = array.size() - 1; i > 0; i--) {
		isSwitched = false;		//initialize the flag to false
		for (int j = 0; j < i; j++) {
			if(array[j] > array[j+1]) {		//switch elements when the former element is bigger than the latter element in the array.
				temp = array[j+1];
				array[j+1] = array[j];
				array[j] = temp;
				isSwitched = true;
			}
		}
		isSwitched ? continue : break;		//finish the loop if nothing has been changed
	}
	return 0;
}
```
<br><br>
If the array has been already sorted(the best case), the complexity of the code above will be (n-1) = **O(n)**. On the other hand, if the last element of the array is the minimum value(the worst case), the complexity of it will be the same as the first algorithm, which is **O(n<sup>2</sup>)**.