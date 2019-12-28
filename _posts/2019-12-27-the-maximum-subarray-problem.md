---
layout: post
title: "The Maximum-Subarray Problem"
author: "DAEUN"
---

Problem: The stock price of a Chemical Corporation is volatile. Let's assume that you know how much the price of the stock will be in the future. Now you are allowed to buy one unit of stock only one time a day and sell it on other days. Your goal is to maximize your profit. When will you buy and sell the stock?

* There is an array of numbers that are the difference between the stock price of the day before current day and current stock price. (ie. (stock price on day4) - (stock price on day5))

### Solution1: Brute-Force Algorithm

You can solve this problem using Brute-Force algorithm. This can be written as below.

```c++
int maxSubArray(int[] A) {
	int sum, i, j, low, high;
    int max = INT.MIN_VALUE;			//lowest int value in max

	for(i = 0; i < A.length; i++) {			//i indicates array index
		sum = 0;			//initialize sum to 0
		for(j = i; j < A.length; j++) {			//j indicates following indices
			sum += A[j];			//add value to sum
			if(sum > max) {			//if sum is greater than max
				max = sum;			//set max to sum
				low = i;
				high = j;
			}
		}
	}
    
	return sum;			//return the highest contiguous sum
}
```

As you can see from the code, variable i indicates the starting index and j indicates the following indices after i. It checks all the elements in array A and adds the value to sum. If the value of sum is greater than max, the value of max will be set to the value of sum and the value of i and j will be saved to the variable low and high.

Since two for-loops each iterate the size of the array, A.length, times. If you say the size of the array is n, this code will execute for-loop for n<sup>2</sup> times. Therefore, the complexity of this code is O(n<sup>2</sup>).
<br><br>
### Solution2: Divide-and-Conquer Algorithm

Divide-and-Conquer algorithm is first splitting the data to small pieces, executing each pieces using recursive function, and combining them. The code is as follow:

```c++
int maxSubarray(int[] A, int low, int high) {
	int mid, leftSum, rightSum, midSum;

	if(low == high) return A[low];			//when there is 1 element
	else {			//when there are 2 or more elements
		mid = (low + hight)/2;

		leftSum = maxSubarray(A, low, mid);			//highest sum in left
		rightSum = maxSubarray(A, mid+1, high);			//highest sum in right
		midSum = maxCrossingSubarray(A, low, mid, high);			//highest sum crossing middle

		if(leftSum >= rightSum && leftSum >= midSum)			//leftSum is the highest among 3 values
			return leftSum;
		else if(rightSum >= leftSum && rightSum >= midSum)			//rightSum is the highest among 3 values
			return rightSum;
		else 			//midSum is the highest among 3 values
			return midSum;
	}
}

int maxCrossingSubarray(int[] A, int low, int mid, int high) {
	int leftSum, rightSum = INT.MIN_VALUE;
	int sum = 0;			//set the sum to 0

	for(int i = mid; i > low; i--) {			//starting from mid, decrease
		sum += A[i];			//add the value to sum
		if(sum > leftSum)
			leftSum = sum;			//reset leftSum if sum is greater than leftSum
	}

	sum = 0;			//reset the value to 0
	for(int j = mid + 1; i < high; i++) {
		sum += A[j];
		if(sum > rightSum)
			rightSum = sum;
	}

	return leftSum + rightSum;			//return total sum
}
```

First we checked if there is one or no element in the array. If there are more than one element in A, we are ready to use divide-and-conquer algorithm here. To divide the array into small subarrays, we need a point where we can split the data into two subarrays. Since splitting the array at the middle is the most efficient, we made a variable called _mid_ and put the middle value between _low_ and _high_. Then we recalled the same function _maxSubarray()_ to left and right subarray and put the highest result into _leftSum_ and _rightSum_. Since these two executions didn't consider contiguous number both crossing left and right side of the array, we called _maxCrossingSubarray()_. This method returns the biggest value of sum. The varialbe i start iteration from _mid_ and in descreases its value after checking if the total sum is bigger than _leftSum_ or not. The varialbe j is doing the same job for the right side of _mid_.

When there is only one or no element in the array, _maxSubarray()_ will return A[low] so that the spent time will be &Theta;(1), and we can write T(1) = &Theta;(1) where parameter of T() is the number of array elements. If there are more than one element in the array, the complexity of this algorithm will be T(n) = 2 * T(n/2) + &Theta;(n). It takes 2 * T(n/2) times since it calls _maxSubarray()_ recursively twice with the half range(low to mid and mid+1 to high). Also, &Theta;(n) time is needed since _maxCrossingSubarray()_ reaches the array n times

reference: T.H. Cormen, C.E. Leiserson, R.L. Rivest, and C. Stein, _Introduction to Algorithms_, 3rd ed. MIT Press, 2009.