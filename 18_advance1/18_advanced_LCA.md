## Advanced 1 LCA

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

## Q5.1 Two nodes in Binary Tree
```java
/*Recursion
Lowest Common Ancestor I
Given two nodes in a binary tree, find their lowest common ancestor.

Assumptions
There is no parent pointer for the nodes in the binary tree
The given two nodes are guaranteed to be in the binary tree

Examples

        5

      /   \

     9     12

   /  \      \

  2    3      14

The lowest common ancestor of 2 and 14 is 5
The lowest common ancestor of 2 and 9 is 9
*/
public class LCAI {
	  // 0. keypoint
	  // two nodes, binary tree, lca
	  
	  // 1. assumption
	  // no parent pointer in the node;
	  // two nodes are guaranteed to be in the tree
	  
	  // 2. approach
	  // recursion, dfs
	  // base case : if root is null/one/two, return root
	  // recursion rule: 
	  // case1: left != null || right != null return left or right
	  // case2: left && right != null || return root
	  // case3: left,right == null return null
	  
	  // 3. data structure
	  
	  // 4. comments
	  
	  // 5. big o complexity
	  // time: O(n) at most we need to loop throgh all the nodes
	  // space: O(height) the depth of call stack is equal to the tree height;
	  public TreeNode lowestCommonAncestor(TreeNode root,
	      TreeNode one, TreeNode two) {
	    // base cases
	    if (root == null || root == one || root == two) {
	      return root;
	    }
	    
	    // get value from left and right child
	    TreeNode left = lowestCommonAncestor(root.left, one, two);
	    TreeNode right = lowestCommonAncestor(root.right, one, two);
	    
	    // three cases
	    if (left != null && right != null) {
	      return root;
	    } else {
	      return left == null ? right : left;
	    }
	  }
}
```

### Q5.2 Two Node in Binary Tree (with parent pointers)
```java
/*Lowest Common Ancestor II
Given two nodes in a binary tree (with parent pointer available), 
find their lowest common ancestor.

Assumptions
There is parent pointer for the nodes in the binary tree
The given two nodes are not guaranteed to be in the binary tree

Examples

        5

      /   \

     9     12

   /  \      \

  2    3      14

The lowest common ancestor of 2 and 14 is 5
The lowest common ancestor of 2 and 9 is 9
The lowest common ancestor of 2 and 8 is null (8 is not in the tree)*/

public class LCAII {
	  // 0. keypoint
	  // 2 nodes , binary tree , with parent pointer
	  
	  // 1. assumption
	  // two nodes are not guaranteed to be in the tree
	  
	  // 2. approach
	  // start from each node, get the height of each node
	  // diff = |heightOne - heightTwo|
	  // move the deeper one up; when diff == 0, move two nodes up together
	  // until they meets
	  
	  // 3. data structure
	  
	  // 4. comment
	  
	  // 5. big o complexity
	  // time: O(n) to get height O(n) to merge
	  // space: O(1)
	  public TreeNodeP lowestCommonAncestor(TreeNodeP one, TreeNodeP two) {
	    int heightOne = getHeight(one);
	    int heightTwo = getHeight(two);
	    
	    TreeNodeP res = mergeNode(one, two, heightOne, heightTwo);
	    
	    return res;
	  }
	  
	  private TreeNodeP mergeNode(TreeNodeP one, TreeNodeP two, int h1, int h2) {
	    int diff = Math.abs(h1 - h2);
	    while (diff != 0) {
	      if (h1 > h2) {
	        one = one.parent;
	        diff--;
	      } else {
	        two = two.parent;
	        diff--;
	      }
	    }
	    
	    // if nodes are not in the same tree
	    // in the end both of them equal to null
	    // return null
	    while (one != two) {
	      one = one.parent;
	      two = two.parent;
	    }
	    
	    return one;
	  }
	  
	  private int getHeight(TreeNodeP node) {
	    int height = 1;
	    while (node != null) {
	      height++;
	      node = node.parent;
	    }
	    return height;
	  }
}
```

### Q5.3 Two nodes in Binary Tree (without parent pointer) (nodes are not guaranteed to be in the tree)
```java
/*Recursion
Lowest Common Ancestor III
Given two nodes in a binary tree, find their lowest common ancestor 
(the given two nodes are not guaranteed to be in the binary tree).

Return null If any of the nodes is not in the tree.

Assumptions
There is no parent pointer for the nodes in the binary tree
The given two nodes are not guaranteed to be in the binary tree

Examples

        5

      /   \

     9     12

   /  \      \

  2    3      14

The lowest common ancestor of 2 and 14 is 5

The lowest common ancestor of 2 and 9 is 9

The lowest common ancestor of 2 and 8 is null (8 is not in the tree)*/

public class LCAIII {
	  // 0. key points
	  // 2 nodes in binary tree without parent pointer
	  // not guaranteed to be in the binary tree
	  
	  // 1. assumption
	  
	  // 2. approach
	  // first traverse the tree to see both nodes are in the tree
	  // do it like a normal tree
	  
	  // 3. data structure
	  
	  // 4. comments
	  
	  // 5. big o complexity
	  // time: O(n) confirm if both nodes are exist, O(n) to find the lca
	  // space: O(height)
	  public TreeNode lowestCommonAncestor(TreeNode root,
	      TreeNode one, TreeNode two) {
	    
	    boolean[] isExists = new boolean[2];
	    exist(root, one, two, isExists);
	    if (isExists[0] == false || isExists[1] == false) {
	      return null;
	    }
	    
	    TreeNode res = lca(root, one, two);
	    
	    return res;
	  }
	  
	  private TreeNode lca(TreeNode root, TreeNode one, TreeNode two) {
	    //base case
	    if (root == null || root == one || root == two) {
	      return root;
	    }
	    
	    TreeNode left = lca(root.left, one, two);
	    TreeNode right = lca(root.right, one, two);
	    
	    if(left != null && right != null) {
	      return root;
	    } else {
	      return left == null? right : left;
	    }
	  }
	  
	  private void exist(TreeNode root, TreeNode one, TreeNode two, boolean[] isExists) {
	    //base case 
	    if (root == null) {
	      return;
	    }
	    
	    if (root == one) {
	      isExists[0] = true;
	    }
	    if (root == two) {
	      isExists[1] = true;
	    }
	    
	    exist(root.left, one, two, isExists);
	    exist(root.right, one, two, isExists);
	  }
}
```

### Q5.4 K nodes in binary tree
```java

/*Lowest Common Ancestor IV
Given K nodes in a binary tree, find their lowest common ancestor.

Assumptions
K >= 2
There is no parent pointer for the nodes in the binary tree
The given K nodes are guaranteed to be in the binary tree

Examples

        5

      /   \

     9     12

   /  \      \

  2    3      14

The lowest common ancestor of 2, 3, 14 is 5

The lowest common ancestor of 2, 3, 9 is 9*/

public class LCAIV {
	  // 0. key point
	  // k nodes in a binary tree
	  
	  // 1. assumption
	  // nodes are guaranteed in the binary tree
	  
	  // 2. approach
	  // recursion
	  // base case: if root == null || anyone of nodes list return root
	  // recursion rule: 
	  // case1: both of left and right is null, return null
	  // case2: both of left and right is not null, return root
	  // case3: either left or right is not null, return not null;
	  
	  // 3. data structure
	  // hash set to store k nodes
	  // to improve the operation of searching
	  
	  // 4. comments
	  
	  // 5. big o complexity
	  // time: O(n) to traverse all nodes
	  // space: O(height) + O(k) (hashset)
	  public TreeNode lowestCommonAncestor(TreeNode root, List<TreeNode> nodes) {
	    Set<TreeNode> nodesSet = new HashSet<>();
	    for (TreeNode t : nodes) {
	      nodesSet.add(t);
	    }
	    
	    TreeNode res = kLCA(root, nodesSet);
	    
	    return res;
	  }
	  
	  private TreeNode kLCA(TreeNode root, Set<TreeNode> set) {
	    // base case
	    if (root == null || set.contains(root)) {
	      return root;
	    }
	    
	    TreeNode left = kLCA(root.left, set);
	    TreeNode right = kLCA(root.right, set);
	    
	    // recursion rule
	    if (left != null && right != null) {
	      return root;
	    } else {
	      return left == null? right : left;
	    }
	  }
}
```
