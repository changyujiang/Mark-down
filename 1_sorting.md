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
    
