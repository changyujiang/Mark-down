## Recursion

[TOC]

What is recursion?

1. 表象上：function calls itself
2. 实质上：Boid down a big problem to smaller one (size n depends on size n-1 or n-2 ... or n/2)
3. Implementation上：
   1. Base case: smallest problem to solve
   2. Recursive rule: how to make the problem smaller (if we can resolve the same problem but with a smaller size, then what is left to do for the current problem size n)



### S1 Recursion 与计算的结合

#### Q1.1 a^b

```java
public long power(int a, int b) {
  // base case
  if (b == 0) {
    return 1L;
  }
  if (b == 1) {
    return (long) a;
  }
  
  long half = power(a, b/2);
  if (b % 2 == 0) {
    return half * half;
  } else {
    return half * half * a;
  }
}
```



#### Q1.2 Power of 2

```java
package laicode.class11.recursion2;

/*Power Of Two
Determine if a given integer is power of 2.

Examples
16 is power of 2 (2 ^ 4)
3 is not
0 is not
-1 is not
*/

public class PowerOf2 {
	// 0. key point
	// input: integer
	// output: boolean

	// 1. assumption
	//

	// 2. approach
	// recursion
	// base case: num == 0 || num == 1 || num == 2
	// recursion rule:
	// isPowerOfTwo(n) = isPowerOfTwo(n/2)

	// 3. data structure

	// 4. comments

	// 5. big o complexity
	// time: O(lgn)
	// space: O(lgn)

	public boolean isPowerOfTwo(int number) {
		// base case
		if (number == 0) {
			return false;
		}
		if (number == 1 || number == 2) {
			return true;
		}

		if (number % 2 == 1) {
			return false;
		}
		return isPowerOfTwo(number / 2);
	}
}

```

### S2 Recursion 与 1D OR 2D Array的结合

1. 1D array: 二分法比较常见
   1. MergeSort
   2. QucikSort
2. 2D array: 
   1. 逐层递归(row by row): 8 queen --> n queens

#### Q2.1 N queens

![image](https://user-images.githubusercontent.com/20658123/33798137-72921962-dcc8-11e7-9090-6411fd591c9d.png)

​							root

​						/ | | | | | | \

​		(Q0,0th-col)   (Q0,1th-col)  (Q0,2th-col) (Q0,3th-col) ... (Q0,7th-col)

​		/|||||||\

(Q1,0th-col) (Q1,1th-col) (Q1,2th-col)  ...........................

Overall # of nodes in recursion tree 1 + 8 + 8^2 + ... 8^8 = O(n^n)

```java
package laicode.class11.recursion2;

import java.util.ArrayList;
import java.util.List;

/*N Queens
Get all valid ways of putting N Queens on an N * N chessboard so that 
no two Queens threaten each other.

Assumptions
N > 0

Return
A list of ways of putting the N Queens
Each way is represented by a list of the Queen's y index for x indices 
of 0 to (N - 1)

Example
N = 4, there are two ways of putting 4 queens:
[1, 3, 0, 2] --> the Queen on the first row is at y index 1, the Queen on the 
second row is at y index 3, the Queen on the third row is at y index 0 and 
the Queen on the fourth row is at y index 2.

[2, 0, 3, 1] --> the Queen on the first row is at y index 2, the Queen on the 
second row is at y index 0, the Queen on the third row is at y index 3 and the 
Queen on the fourth row is at y index 1.*/

public class NQueens {
	// 0. key point
	// put N queens on N*N chessboard
	// return all valid way

	// 1. assumption
	// N > 0

	// 2. approach
	// Recursion DFS to go through all possible ways to put the queens
	// base case: on the last row
	// 					root
	// 				/ | | | | | | \
	// 		(Q0,0th-col) (Q0,1th-col) (Q0,2th-col) (Q0,3th-col) ... (Q0,7th-col)
	// 		  /|||||||\
	// (Q1,0th-col) (Q1,1th-col) (Q1,2th-col) ...........................
	// Overall # of nodes in recursion tree 1 + 8 + 8^2 + ... 8^8 = O(n^n)

	// 3. data structure

	// 4. comments

	// 5. big o complexity
	// time: O(n^n)
	// space: O(n) height of recursion tree
	public List<List<Integer>> nqueens(int n) {
		// n>=1
		List<List<Integer>> res = new ArrayList<>();
		List<Integer> cur = new ArrayList<>();

		int initRow = 0;
		nqueens(n, initRow, res, cur);

		return res;
	}

	private void nqueens(int n, int curRow, List<List<Integer>> res,
			List<Integer> cur) {
		if (curRow == n) {
			res.add(new ArrayList<Integer>(cur));
			return;
		}

		for (int i = 0; i < n; i++) {
			if (validOnCol(i, curRow, cur)) {
				cur.add(i);
				nqueens(n, curRow + 1, res, cur);
				cur.remove(cur.size() - 1);
				// cur.remove(i + 1);
			}
		}
	}

	private boolean validOnCol(int col, int row, List<Integer> cur) {
		if (cur.size() == 0) {
			return true;
		}
		// i row
		// j col
		for (int i = 0; i < cur.size(); i++) {
			int j = cur.get(i);
			int rowOffset = Math.abs(row - i);
			if (col == j || col == j + rowOffset || col == j - rowOffset) {
				return false;
			}
		}
		return true;
	}
}

```



#### Q2.2 How to print 2D array in spiral order (N * N)

```java
package laicode.class10;

import java.util.ArrayList;
import java.util.List;

/*Spiral Order Traverse I
Traverse an N * N 2D array in spiral order clock-wise starting 
from the top left corner. Return the list of traversal sequence.

Assumptions
The 2D array is not null and has size of N * N where N >= 0

Examples
{ {1,  2,  3},

  {4,  5,  6},

  {7,  8,  9} }

the traversal sequence is [1, 2, 3, 6, 9, 8, 7, 4, 5]*/

public class SpiralI {
	// 2. approach
	// 2.2 iterative
	public List<Integer> spiralI(int[][] matrix) {
		List<Integer> res = new ArrayList<>();
		int start = 0;
		int end = matrix.length - 1;
		// termination condition: start = end - 1 || start = end
		while (start < end) {
			for (int i = start; i < end; i++) {
				res.add(matrix[start][i]);
			}
			for (int i = start; i < end; i++) {
				res.add(matrix[i][end]);
			}
			for (int i = end; i > start; i--) {
				res.add(matrix[end][i]);
			}
			for (int i = end; i > start; i--) {
				res.add(matrix[i][start]);
			}
			start++;
			end--;
		}
		if (start == end) {
			res.add(matrix[start][start]);
		}
		return res;
	}

	// 0. KEY POINT
	// N*N matrix
	// traverse in spiral order clock-wise
	// return list of sequence

	// 1. assumption
	// array is not null
	// N >= 0

	// 2. approach
	// 2.1 recursion
	//
	// 2.2 iterative

	// 3. data sturcutre

	// 4. comments

	// 5. BIG O complexity
	// time: O(n^2)
	// space: O(n)
	public List<Integer> spiral(int[][] matrix) {
		// assumption / corner case
		List<Integer> res = new ArrayList<>();
		if (matrix.length == 0) {
			return res;
		}

		// n >= 1
		int size = matrix.length;
		int offset = 0;
		spiral(matrix, res, offset, size);
		return res;
	}

	// example:
	// 0 1 2

	// 1, 2, 3
	// 4, 5, 6
	// 7, 8, 9
	// size = 3
	// size-1 = 2

	// size >= 1
	private void spiral(int[][] matrix, List<Integer> res, int offset, int size) {
		// base case
		if (size == 0) {
			return;
		}
		if (size == 1) {
			res.add(matrix[offset][offset]);
			return;
		}
		// i < 2,
		// up row
		for (int i = 0; i < size - 1; i++) {
			res.add(matrix[offset][i + offset]);
		}
		// right col
		for (int i = 0; i < size - 1; i++) {
			res.add(matrix[i + offset][size - 1 + offset]);
		}
		// bottom row
		for (int i = 0; i < size - 1; i++) {
			res.add(matrix[size - 1 + offset][size - 1 + offset - i]);
		}
		// left col
		for (int i = 0; i < size - 1; i++) {
			res.add(matrix[size - 1 + offset - i][offset]);
		}

		spiral(matrix, res, offset + 1, size - 2);
	}
}

```



### S3 Recrusion 与 LinkedList结合

#### Q3.1 Reverse a linked list (Class3 review)

![image](https://user-images.githubusercontent.com/20658123/33801356-10479926-dd0e-11e7-99d0-83edf8ed6cab.png)

#### Q3.2 Reverse a linkedlist (Pair by Pair)

E.G.

input: 1-> 2-> 3-> 4-> 5-> null

output: 2-> 1-> 4-> 3-> 5-> null

1 -> 2  | 3 4 5 null

subproblem 3 4 5 null

head = cur.next;

head.next = cur;

cur.next = reverse(cur.next.next);

1 <- 2

![image](https://user-images.githubusercontent.com/20658123/33801364-6506c81a-dd0e-11e7-88cf-9319bd841bb5.png)

```java
package laicode.class10;

import laicode.class3.ListNode;

/*Reverse Linked List In Pairs
Reverse pairs of elements in a singly-linked list.

Examples

L = null, after reverse is null
L = 1 -> null, after reverse is 1 -> null
L = 1 -> 2 -> null, after reverse is 2 -> 1 -> null
L = 1 -> 2 -> 3 -> null, after reverse is 2 -> 1 -> 3 -> null*/

public class ReverseInPairs {
	// 0. key point
	// reverse pair by pair

	// 1. assumption
	// if input is null, return null
	// if input.next == null, return input

	// 2. approach
	// recursion
	// 1 -> 2 -> 3 -> null
	// subproblem:
	// subHead = reverse(head.next.next);
	// 2 -> 1 -> 3 -> null

	// 3. data structure

	// 4. comment

	// 5. BIG O COMPLEXITY
	// TIEM: O(N)
	// SPACE: O(N/2) call stack
	public ListNode reverseInPairs(ListNode head) {
		// base case
		if (head == null || head.next == null) {
			return head;
		}

		ListNode subHead = reverseInPairs(head.next.next);
		ListNode newHead = head.next;
		newHead.next = head;
		head.next = subHead;

		return newHead;
	}
}
```



### S4 Recursion 与 String的结合

#### Q4.1 Reverse a string using recursion

abcd -> dcba

#### Q4.2 String Abbreviation Matching 

A word such as "book" can be abbreviated to 4, 1o1k, b3, b2k, etc. Given a string and an abbreviation, return if the string matches the abbreviation. Assume the original string only contains alphabetic characters. For example: "s11d" matches "sophisticated"

```java
package laicode.class10.recursion2;

/*String Abbreviation Matching
 Word “book” can be abbreviated to 4, b3, b2k, etc. Given a string and an 
 abbreviation, return if the string matches the abbreviation.

 Assumptions:
 The original string only contains alphabetic characters.
 Both input and pattern are not null.

 Examples:
 pattern “s11d” matches input “sophisticated” since “11” matches eleven 
 chars “ophisticate”.*/

public class StringAbbreviationMatching {
	// 0. key point
	// match a string and abbreviation
	// return a boolean

	// 1. assumption
	// string only contains alphabetic characters
	// both are not null
	// length >= 0

	// 2. approach
	// recursion
	// case1: pattern.charAt(i) == 'a-zA-Z', check with input.charAt(j)
	// case2: pattern.charAt(i) == '0-9', read the number and move pointers on
	// input

	// 3. data structure

	// 4. comment

	// 5. big o complexity
	// Time: O(n)
	// Space: O(m)
	public boolean match(String input, String pattern) {
		// assumption: not null
		// corner case
		if (input.length() == 0 || pattern.length() == 0) {
			return input.length() == 0 && pattern.length() == 0 ? true : false;
		}

		// pointers for each string
		int p0 = 0;
		int p1 = 0;
		return match(input, pattern, p0, p1);
	}

	private boolean match(String input, String pattern, int i, int j) {
		// base case
		if (i == input.length() && j == pattern.length()) {
			return true;
		}
		if (i >= input.length() || j >= pattern.length()) {
			return false;
		}

		// case 1
		// not a digit
		if (pattern.charAt(j) > '9' || pattern.charAt(j) < '0') {
			if (pattern.charAt(j) == input.charAt(i)) {
				return match(input, pattern, i + 1, j + 1);
			}
			return false;
		}

		// case2
		int num = 0;
		while (j < pattern.length() && pattern.charAt(j) <= '9'
				&& pattern.charAt(j) >= '0') {
			num = num * 10 + pattern.charAt(j) - '0';
			j++;
		}
		return match(input, pattern, i + num, j);
	}
}

```



### S5 Recursion 与 Tree的结合: 第一类从下往上返值

* Binary tree 往往是最常见的，和recursion 结合最紧密的面试题目类型。

* Reasons:

  * 每层的node具备的性质，传递的值和下一层的性质往往一致。比较容易定义 recursion rule。

  * Base case (generally): null pointer under leaf node (sometimes, left node)

  * E.g. int getHeight (Node root)

  * E.g. 统计tree中有多少个node？

    ![image](https://user-images.githubusercontent.com/20658123/33808358-b2dc3fe4-dd99-11e7-82d2-220529f11203.png)



 #### Q5.1 getHeight

```java
public int getHeight(TreeNode root) {
  //base case
  if (root == null) {
    return 0;
  }
  
  int left = getHeight(root.left);
  int right = getHeight(root.right);
  
  return Math.max(left, height) + 1;
}
```



#### Q5.2 Store the number of nodes in each node's left-subtree

* **Way of thinking(Tricks)** 

  1. What do you expect from your lchild/ rchild? (usually it is the return type of recursion function)

     * total number of nodes in my left substree
     * total number of nodes in my right substree 



    2. What do you want to do in the current layer?

     * store leftNum in current.lChildNum

    3. What do you want to report to your parent?

     * return leftNum + rightNum + 1;


```java
class TreeNode {
  int value;
  TreeNode left;
  TreeNode right;
  int lChildNum;
}

public int numLeftTree(TreeNode root) {
  //base case
  if (root == null){
    return 0;
  }
  
  // 
  int left = numLeftTree(root.left);
  int right = numLeftTree(root.right);
  root.lChildNum = left;
  return left + right + 1;
}
```

#### Q5.3 Find the node with max difference in the totoal number of descendents in its left subtree and right subtree

Way of thinking:

1. what do you expect from your lchild/ rchld? (usually its the return type of the recursion function)

   * left: number of descendents of left subtree
   * right: number of descendents of right subtree

2. what do you want to do in the current layer?

   * check whether Math.abs(left - right) > global_max_diff
   * if so, update it to global_max_diff

3. what do you want to report to your parent?

   * return 1+left+right

   ```java
   public int maxDiff (TreeNode root, int[] maxDiff) {
     // base case
     if (root == null) {
       return 0;
     }
     
     // recursion rule
     int left = maxDiff(root.left);
     int right = maxDiff(root.right);
     if (Math.abs(left - right) > maxDiff[0]) {
       maxDiff[0] = Math.abs(left - right);
     }
     return left + right + 1;
   }
   ```

   ​

