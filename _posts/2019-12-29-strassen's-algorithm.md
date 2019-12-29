---
layout: post
title: "Strassen's Algorithm for Matrix Multiplication"
author: "DAEUN"
---

Problem: Matrix Multiplication

Let's assume square n x n matrix A, B, and C where C is a result of multiplication between A and B.

### Solution1: Naive Algorithm

```c++
void matrixMsultiplication(int A[SIZE][SIZE], int B[SIZE][SIZE]) {
	int C[][SIZE] = {};					//result matrix C
	for(int i = 0; i < SIZE; i++) {		//iterate until the end of row
		for(int j = 0; j < SIZE; j++) {	//iterate until the end of column
			C[i][j] = 0;				//initialize matrix C to 0
			for(int k = 0; k < SIZE; k++) {	//iterate k
				C[i][j] += A[i][k]*B[k][j];	//multiply numbers
			}
		}
		printf("%d ", C[i][j]);			//print the result
	}
}
```

Since this code iterates each for-loops for SIZE times, which is the size of the matrix n, the complexity of naive algorithm is &Theta;(n<sup>3</sup>) times.
<br><br>
### Solution2: Divide-and-Conquer

```c++
void matrixMultiplication2(int A[SIZE][SIZE], B[SIZE][SIZE]) {
	int n = A.rows;
	int C[][SIZE] = { };
	if(n == 1) C[1][1] = A[1][1]*B[1][1];
	else {
		C[1][1] = matrixMultiplication2(A[1][1], B[1][1]) + matrixMultiplication2(A[1][2], B[2][1]);
		C[1][2] = matrixMultiplication2(A[1][1], B[1][2]) + matrixMultiplication2(A[1][2], B[2][2]);
		C[2][1] = matrixMultiplication2(A[2][1], B[2][2]) + matrixMultiplication2(A[2][1], B[1][1]);
		C[2][2] = matrixMultiplication2(A[2][1], B[1][2]) + matrixMultiplication2(A[2][2], B[2][2]);
	}
}
```

When n is equal to 1, T(1) = &Theta;(1) time is needed because of the if statement of the code above. When n is greater than 1, however, it will take T(n) = 8\*T(n/2) + &Theta;(n<sup>2</sup>) time since recursive function gets n/2 sub matrices as parameters. As the above code executes matrixMultiplication2() recursively, we can get the complexity by putting the right value to T(n/2) recursively like this:

T(n) = 8\*T(n/2) + &Theta;(n<sup>2</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>2</sup>\*T(n/2<sup>2</sup>) + &Theta;(n<sup>2</sup>/2<sup>2</sup>) + &Theta;(n<sup>2</sup>/2<sup>0</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>3</sup>\*T(n/2<sup>3</sup>) + &Theta;(n<sup>2</sup>/2<sup>4</sup>) + &Theta;(n<sup>2</sup>/2<sup>2</sup>) + &Theta;(n<sup>2</sup>/2<sup>0</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ...<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>k</sup>\*T(n/2<sup>k</sup>) + &Theta;(n<sup>2</sup>/2<sup>2(k-1)</sup>) + ... + &Theta;(n<sup>2</sup>/2<sup>2</sup>) + &Theta;(n<sup>2</sup>/2<sup>0</sup>)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>lgn</sup>\*T(1) + &Theta;(n<sup>2</sup>\*(4n/3 - 8/3n)) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (&because; n/2<sup>k</sup> = 1)<br>
&nbsp; &nbsp; &nbsp; &nbsp; = 8<sup>lgn</sup>\*&Theta;(1) + &Theta;(n<sup>3</sup>) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (&because; T(n) = &Theta;(1))<br>
&therefore; the complexity of divide-and-conquer algorithm is O(n<sup>3</sup>)

<br><br>
### Solution3: Strassen's Algorithm

```c++
M1: A[1][1]*(B[1][2]-B[2][2])
M2: (A[1][1]+A[1][2])*B[2][2]
M3: (A[2][1]+A[2][2])*B[1][1]
M4: A[2][2]*(B[2][1]-B[1][1])
M5: (A[1][1]+A[2][2])*(B[1][1]+B[2][2])
M6: (A[1][2]-A[2][2])*(B[2][1]+B[2][2])
M7: (A[1][1]-A[2][1])*(B[1][1]+B[1][2])

C[1][1] = M5 + M4 - M2 + M6			//output value
C[1][2] = M1 + M2
C[2][1] = M3 + M4
C[2][2] = M5 + M1 - M3 - M7
```

This algorithm is found by a German Mathematician, Volker Strassen. It multiplies elements of matrices 7 times instead of 8 times. Therefore, the complexity of Strassen's Algorithm is T(n) = 7\*T(n/2) + &Theta;(n<sup>2</sup>), which takes 1\*T(n/2) time less than the complexity of division-and-conquer algorithm. As a result, the complexity of Strassen's Algorithm is &Theta;(n<sup>lg7</sup>). You can get this result by substituting T(n/2) recursively or using method for solving recurrences that we will learn next time.
<br><br><br>
reference: T.H. Cormen, C.E. Leiserson, R.L. Rivest, and C. Stein, _Introduction to Algorithms_, 3rd ed. MIT Press, 2009.