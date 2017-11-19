## Sorting Algorithms

1. what are sorting algorithms?
    * selection sort
    * merge sort
    * quick sort
    * bubble sort
    * patient sort
    * smooth sort
    * cocktail sort
    ...

2. what are they good for?
    * combined with system design

> 做题要求
> A complete answer will include the following:
> 1. Document your assumption
> 2. Explain your approach and how you intent to solve the problem
> 3. Provide code comment where applicable
> 4. explain the big-O complexity of your solution. justify your answer
> 5. indentity any additional data structure you used and justify why you used them.
> 6. only provide your best answer to each part of the question.

### Q1 Selection sort

```java
/*Selection Sort
Given an array of integers, sort the elements in the array in ascending order. 
The selection sort algorithm should be used to solve this problem.

Examples:
{1} is sorted to {1}
{1, 2, 3} is sorted to {1, 2, 3}
{3, 2, 1} is sorted to {1, 2, 3}
{4, 2, -3, 6, 1} is sorted to {-3, 1, 2, 4, 6}

Corner Cases:
What if the given array is null? 
	In this case, we do not need to do anything.
What if the given array is of length zero? 
	In this case, we do not need to do anything.*/

public class SelectionSort {
	public int[] selectionSort(int[] array) {
	    // 1. assumption: 
	    // if array is null or empty, return array;
	    // 2. approach:
	    // selection sort, 
	    // divide the input array into two parts;
	    // sorted and unsorted, say left and right
	    // each iteration we selected a minimum item from the unsorted part
	    // and put it to sorted part
	    
	    // corner case
	    if (array == null || array.length == 0) {
	      return array;
	    }
	    // demo 
	    // {4, 2, -3, 6, 1} 
	    //  i
	    // {-3, 1, 2, 4, 6}
	    for (int i = 0; i < array.length; i++) {
	      // find min from unsorted part
	      int minIndex = i;
	      for (int j = i + 1; j < array.length; j++) {
	        if (array[j] < array[minIndex]) {
	          minIndex = j;
	        }
	      }
	      // swap it to sorted part
	      swap(array, i, minIndex);
	    }
	    return array;
	  }
	  
	  private void swap(int[] array, int i, int j) {
	    int temp = array[i];
	    array[i] = array[j];
	    array[j] = temp;
	    return;
	  }
	  // 3. complexity
	  // time: there are n iterations for a array with length to sort the whole 
	  // array, and for each iteration, we need at most n comparsion operations;
	  // so, the time complexity is O(n^2)
	  // space: we do it in-place O(1)
}
```

### Q1.1 Use stack to simulate selection sort
> Given an array stored in stack1, how to sort the numbers by using additional two stacks? What if one additional 
stack can be use?

Stack1 || 3 4 2     int global_min = 1  
Stack2 || 1  
Stack3 ||  

Solution for two stacks:
1. stack2 can be used as buffer
2. stack3 can be used to store the final result
3. maintain a global_min
    
### Q2 Merge Sort

```java
/*Merge Sort
Given an array of integers, sort the elements in the array in ascending order. 
The merge sort algorithm should be used to solve this problem.

Examples:
{1} is sorted to {1}
{1, 2, 3} is sorted to {1, 2, 3}
{3, 2, 1} is sorted to {1, 2, 3}
{4, 2, -3, 6, 1} is sorted to {-3, 1, 2, 4, 6}

Corner Cases:
What if the given array is null? 
	In this case, we do not need to do anything.
What if the given array is of length zero? 
	In this case, we do not need to do anything.*/

public class MergeSort {
	public int[] mergeSort(int[] array) {
	    // 1.assumption
	    // if array is null or empty, do nothing, return array
	    // 2. approach
	    // merge sort algorithm
	    // each time we divide the array into two parts, until the size is one, say
	    // we can slove the base case in O(1)
	    // then, we merge the sub-answer to build a full answer
	    // 4. complexity
	    // time
	    //                          4 2 -3 6 1
	    //              4 2 -3                        6 1
	    //        4 2         -3                6              1
	    //     4     2        
	    //                              n                        divide O(1) merge O(n)
	    //                  n/2                   n/2            divide O(2) merge O(n)
	    //             n/4       n/4           n/4       n/4     divide O(4) merge O(n)
	    //         n/8    n/8  n/8    n/8   n/8   n/8  n/8  n/8  divide O(8) merge O(n)    
	    //       ....
	    //      1   1     1  1   1  1  ...                  1 1  divide O(n) merge O(n)
	    //  the tree height is logn, total time is O(nlogn)
	    // space
	    // the helper int array O(n)
	    
	    
	    // corner cases
	    if (array == null || array.length == 0) {
	      return array;
	    }
	    // example
	    // 4, 2, -3, 6, 1
	    
	    // helper int array for merge operation
	    int[] helper = new int[array.length];
	    mergeSort(array, helper, 0, array.length - 1);
	    return array;
	  }
	  
	  private void mergeSort(int[] array, int[] helper, int l, int r) {
	    // base case
	    if (l >= r) {
	      return;
	    }
	    // instead of (l + r) / 2, we use following way to avoid overflow
	    int mid = l + (r - l) / 2;
	    // divide into two subproblem;
	    // (l,mid) (mid+1,r); how to divide is important
	    mergeSort(array, helper, l, mid);
	    mergeSort(array, helper, mid + 1, r);
	    // example: 
	    // if l,r = (7, 9) mid = 8, then (l, mid) = (7,8)[2] (mid+1, r) = (9,9)[1]
	    // if l,r = (7, 8) mid = 7, then (l, mid) = (7,7)[1] (mid+1, r) = (8,8)[1]
	    // l = r, return
	    // if l,r = (7, 7) mid = 7, then (l, mid) = (7,7)[1] (mid+1, r) = (8,7)[0]
	    
	    // merge up (l < r)
	    merge(array, helper, l, r, mid);
	  }
	  
	  private void merge(int[] array, int[] helper, int l, int r, int mid) {
	    for (int i = l; i <= r; i++) {
	      helper[i] = array[i];
	    }
	    // init, i is index of left part subarray, j is index of right part subarray
	    int i = l;
	    int j = mid + 1;
	    // cur index
	    int cur = l;
	    while (i <= mid && j <= r && cur <= r) {
	      array[cur++] = helper[i] <= helper[j] ? helper[i++] : helper[j++];
	    }
	    // if we still have some elments in left side, we need to copy them
	    while (i <= mid) {
	      array[cur++] = helper[i++];
	    }
	    // if we have some elments in right side, we dont need to copy them
	    // becasue they are already in their position
	    // example: 
	    // array[]  123 145 
	    //helper[]  123 145
	    // result   112 345
	    //while (j <= r) {
	    //  array[cur++] = helper[j++];
	    //}
	    return;
	  }
}
```