## Advanced 1 剥洋葱

* Table of Content
    * 隔板题(Two/N pointers)
    * Minimize #comparsion
    * 剥洋葱
    * BFS print binary tree
    * LCA

> 做题要求
> A complete answer will include the following:
> 1. Document your assumption
> 2. Explain your approach and how you intent to solve the problem
> 3. Provide code comment where applicable
> 4. explain the big-O complexity of your solution. justify your answer
> 5. indentity any additional data structure you used and justify why you used them.
> 6. only provide your best answer to each part of the question.

## Q3.1
```java
/*
 * Rotate an N * N matrix clockwise 90 degrees.

Assumptions
The matrix is not null and N >= 0

Examples
{ {1,  2,  3}

  {8,  9,  4},

  {7,  6,  5} }

after rotation is

{ {7,  8,  1}

  {6,  9,  2},

  {5,  4,  3} }*/

public class RotateMatrix {
	  // 0. keypoint
	  // N*N matrix; rotate 90 degree
	  // 1. assumption
	  // matrix is not null; N >= 0
	  // if N = 0; do nothing
	  
	  // 2. approach
	  // do the ratate operation laryer by layer from outer to inner
	  // like peel onion
	  
	  // example
	  // 1 2 3
	  // 8 9 4
	  // 7 6 5
	  
	  // [1][1] -> [1][n-1]; [1][2] -> [2][n-1]
	  
	  // 3. comments
	  
	  // 4. big o complexity
	  // time: O(n^2)
	  // space: O(1)
	  
	  // 5. additional data structure
	  public void rotate(int[][] matrix) {
	    if (matrix.length == 0) {
	      return;
	    }
	    int n = matrix.length;
	    // if n = 0,1 round = 0
	    // if n = 2,3 round = 1
	    // if n = 4,5 round = 2
	    int round = n / 2;
	    for (int i = 0; i < round; i++) {
	      int left = i;
	      int right = n - i - 2;
	      for (int j = left; j <= right; j++) {
	        int temp = matrix[left][j];
	        matrix[left][j] = matrix[n - 1 - j][left];
	        matrix[n - 1 - j][left] = matrix[n - 1 - left][n - 1 - j];
	        matrix[n - 1 - left][n - 1 - j] = matrix[j][n - 1- left];
	        matrix[j][n - 1- left] = temp;
	      }
	    }
	  }
}
```
