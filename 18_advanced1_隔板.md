## Advanced 1 Two Pointers

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

* 隔板同向而行，two pointers move in the same direction
    1. 基本思想：we maintain pointers, slow and fast
    2. 性质1： left to slow is what we want to return, right to fast is unexplored, 
    between slow and fast is what we dont care.
    3. 性质2： after the whole process, maintain the relative order of the elements in the array.
    
* 隔板相向而行，two pointers move in opposite direction
    1. 基本思想：we maintain two pointers, left and right
    2. 性质1：left to slow is what we want to return , right to right is what we want to return too
    , between left and right is unexlored area.
    3. 性质2： after the whole process, cannot maintain the relative order.
    
* 注意：
    1. 定义隔板的时候，注意inculsive or exclusive
    2. 同向而行适用于保持相对位置的题目中（可以丢弃item）；
    3. 相向而行适用于不需要保持相对位置题目中（不丢弃item？好像也可以丢弃item）；
    4. 同向而行不一定都是从左往右，也可以考虑从右往左的情况(比如说string replacement?)；
    
### Q1.1
```java
/* Array Deduplication I

Given a sorted integer array, remove duplicate elements. 
For each group of elements with the same value keep only one of them. 
Do this in-place, using the left side of the original array and maintain 
the relative order of the elements of the array. 

Return the array after deduplication.

Assumptions:
The array is not null

Examples:
{1, 2, 2, 3, 3, 3} → {1, 2, 3}*/

/*
 * key points:
 * 		sorted array, it means that duplicate items must be adjacent;
 * 		then, we can use two pointers to scan them in one pass.
 */

public class ArrayDeduplicationI {
	  // 1. assumption: 
	  // the array is not null
	  // if array is empty, return array
	  
	  // 2. approach:
	  // use two pointers in the same direction, slow and fast
	  // left of slow (inclusive) is what we want to return, all item processed
	  // between slow and fast is dont care area
	  // right to fast is unexplored area
	  
	  // in each iteration, we compare array[fast] and array[slow], if they are different
	  // array[++slow] = array[fast++]
	  // otherwise fast++;
	  
	  // 3. comment where it applicable
	  // 4. Big O complexity
	  // time: O(n)
	  // space: O(1)
	  // 5. identity data structure
	  
	  public int[] dedup(int[] array) {
	    // corner case
	    if (array == null || array.length <= 1) {
	      return array;
	    }
	    // init 
	    // slow(inclusive); at least we need to return 1 elment
	    int slow = 0;
	    int fast = 1;
	    // example
	    // 1 2 2 3 3 3 len
	    // s
	    //   f
	    
	    // terminate condition: fast = array.length
	    while (fast < array.length) {
	      if (array[fast] != array[slow]) {
	        array[++slow] = array[fast++];
	      } else {
	        fast++;
	      }
	    }
	    
	    return Arrays.copyOf(array, slow + 1);
	  }
}
```

### Q1.2
```java
public class ArrayDeduplicationII {
	  // 0. key points: 
	  // sorted input array;
	  // keep at most two, in-place, relative order required
	  
	  // 1. assumption:
	  // array is not null;
	  // if array is empty, do nothing
	  
	  // 2. approach:
	  // two pointers, slow and fast, left of slow(inclusive), right of fast
	  // init: slow = 1 (at least we can return two items) fast = 2
	  // each iteration: 
	  // if (array[slow - 1] == array[fast]) fast++;
	  // else array[++slow] = array[fast++];
	  // termination: fast = array.length (all items scaned)
	  
	  // 3. comments where applicable
	  
	  // 4. big O complexity
	  // time: O(n)
	  // space: O(1)
	  
	  //5. additional data structure if necessary
	  
	  public int[] dedup(int[] array) {
	    // corner case
	    if (array.length <= 2) {
	      return array;
	    }
	    
	    // example
	    // 0 1 2 3 4 5 len
	    // 1 2 2 3 3 3
	    //         s
	    //             f
	    // init
	    int slow = 1;
	    int fast = 2;
	    
	    while (fast < array.length) {
	      if (array[fast] != array[slow - 1]) {
	        array[++slow] = array[fast++];
	      } else {
	        fast++;
	      }
	    }
	    
	    return Arrays.copyOf(array, slow + 1);
	  }
}
```

### Q1.3
```java
/* Data Structure
Array Deduplication III

Given a sorted integer array, remove duplicate elements. 
For each group of elements with the same value do not keep any of them. 
Do this in-place, using the left side of the original array and and 
maintain the relative order of the elements of the array. 
Return the array after deduplication.

Assumptions
The given array is not null

Examples
{1, 2, 2, 3, 3, 3} → {1}*/

public class ArrayDeduplicationIII {
	  // 0. keypoints:
	  // sorted array, don't keep any of them,
	  // inplace, realtive order counts
	  
	  // 1. assupmtion:
	  // given array is not null
	  // if array is empty, do nothing
	  
	  // 2. approach:
	  // two pointers, slow and first in the same direction
	  // left of slow(inclusive) is what we want to return
	  // init: slow = -1, we cannot ensure that there must be at least one item returned.
	  // fast = 0;
	  // each iteration we check # of array[f]
	  // example:
	  //  0 1 2 3 4 5 l
	  //  1 2 2 3 3 3
	  //s 
	  //        f
	  //    b
	  // f - b == 1 copy array[b] to array[++slow]
	  // else continue
	  
	  // 3. Comments applicable
	  
	  // 4. Big O complexity
	  // time: one pass O(n)
	  // space: O(1)
	  
	  // 5. additional data structure if necessary
	  public int[] dedup(int[] array) {
	    // corner case
	    if (array.length == 0) {
	      return array;
	    }
	    // init
	    int slow = -1;
	    int fast = 0;
	    while (fast < array.length) {
	      int begin = fast;
	      while (fast < array.length && array[fast] == array[begin]) {
	        fast++;
	      }
	      // if only one item(no duplicate) copy, otherwise continue
	      if (fast - begin == 1) {
	        array[++slow] = array[begin];
	      } 
	    }
	    
	    return Arrays.copyOf(array, slow + 1);
	  }
}
```

## Q1.4
```java
/*Data Structure
 Array Deduplication IV
 Given an unsorted integer array, remove adjacent duplicate elements repeatedly,
 from left to right. 
 For each group of elements with the same value do not keep any of them.

 Do this in-place, using the left side of the original array. 
 Return the array after deduplication.

Assumptions
The given array is not null

Examples
{1, 2, 3, 3, 3, 2, 2} → {1, 2, 2, 2} → {1}, return {1}*/
public class ArrayDeduplicationIV {
	  // 0. keypoints:
	  // unsorted array; adjacent duplicate repeatedly
	  // inplace
	  
	  // 1. assumption:
	  // input is not null
	  // if input is empty, do nothing
	  
	  // 2. approach:
	  // inplicitly stack to do it
	  // two pointers, slow, fast, the left of slow(inclusive) is what we want to return
	  // we use the left part of array as a stack, slow is top of stack
	  // expamle:
	  //  1 2 3 3 3 2 2
	  //  s
	  //                f
	  
	  // 3. comments
	  
	  // 4. Big O complexity
	  // time: one pass O(n)
	  // space: O(1)
	  
	  // 5. additional data structure
	  public int[] dedup(int[] array) {
	    // corner case
	    if (array.length <= 1) {
	      return array;
	    }
	    
	    // init: stack is empty
	    int slow = -1;
	    int fast = 0;
	    
	    while (fast < array.length) {
	      if (slow == -1 || array[slow] != array[fast]) {
	        array[++slow] = array[fast++];
	      } else {
	        while (fast < array.length && array[slow] == array[fast]) {
	          fast++;
	        }
	        slow--;
	      }
	    }
	    return Arrays.copyOf(array, slow + 1);
	  }
}
```

## Q1.5
```java
/*Data Structure
Move 0s To The End II
Given an array of integers, move all the 0s to the right end of the array.

The relative order of the elements in the original array need to be maintained.

Assumptions:
The given array is not null.

Examples:
{1} --> {1}
{1, 0, 3, 0, 1} --> {1, 3, 1, 0, 0}*/
public class Move0sToTheEndII {
	  // 0. keypoint:
	  // relative order counts
	  // 1. assupmiton:
	  // input array is not null
	  // if input is empty, do nothing
	  
	  // 2. approach:
	  // two pointers, slow and fast, the left of slow(inclusive) are all non-zeros,
	  // between slow(exclusive) and fast(exclusive) are zeros
	  // init: slow = -1 fast = 0
	  // termination: fast = array.length
	  // in each iteration: if array[fast] = 0 fast++;
	  // else slow++; swap(slow, fast); fast++
	  
	  // 3. comments
	  
	  // 4. Big O complexity
	  // time: ONE PASS O(N)
	  // space: O(1)
	  
	  // 5. additional data structures
	  public int[] moveZero(int[] array) {
	    // corner case
	    if (array == null || array.length == 0) {
	      return array;
	    }
	    
	    // init
	    int slow = -1;
	    int fast = 0;
	    
	    while (fast < array.length) {
	      if (array[fast] != 0) {
	        slow++;
	        swap(array, slow, fast);
	        fast++;
	      } else {
	        fast++;
	      }
	    }
	    
	    return array;
	  }
	  
	  private void swap(int[] array, int i, int j) {
	    int temp = array[i];
	    array[i] = array[j];
	    array[j] = temp;
	  }	
}
```