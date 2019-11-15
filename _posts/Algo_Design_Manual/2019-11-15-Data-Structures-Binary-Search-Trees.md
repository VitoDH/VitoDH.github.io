# Chapter 3 - Data Structures - 3 - Binary Search Trees

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)



### Def

* Rooted Binary Tree: either empty or consisting of a node called the root, together with two rooted binary trees called the left and right subtrees respectively
* Binary search tree: labels each node in binary tree with a single key such that for any node labeled x, all nodes in the left sub subtree of x have keys < x while all nodes in the right subtree of x have keys > x



### 3.1 Implementing Binary Search Trees



#### Structures

```python
class binaryTree(){
    def __init__(self,val):
		self.val = val
    	self.parent = None    # pointer to parent
    	self.left = None	  # pointer to a left child
    	self.right = None	  # pointer to a right child
}
```



#### Searching in a Tree

```python
# Recursive search algorithm
def searchTree(root,val):
    if root is None:
        return None
    if root.val == val:
        return root
    
    if val < root.val:
        return searchTree(root.left,val)
    else:
        return searchTree(root.right,val)
```

Time: O(h) where h denotes the height of the tree



#### Finding minimum and maximum elements in a tree

* The minimum element must be the leftmost descendent of the root
* The maximum element must be the rightmost descendent of the root

```python
def findMinimum(root):
    if root is None:
        return None
    
    min_ = root
    while min_.left:
        min_ = min_.left
    return min_


def findMaximum(root):
    if root is None:
        return None
    
    max_ = root
    while max_.right:
        max_ = max_.right
    return max_
```





#### Traversal in a tree

A prime application of tree traversal is listing the labels of the tree nodes in sorted order.

We can visit the nodes recursively with an **in-order** traversal.

```python
def traverseTree(root):
    
    if root:
        traverseTree(root.left)
        print(root.val)
        traverseTree(root.right)
```

Time: O(n) where n denotes the number of nodes in the tree. Each node is visited once during the traversal.



#### Insertion in a Tree

this implementation uses recursion to combine the search and node insertion stages of key insertion.

```python
def insertTree(root,val,parent):
    if root is None:
        root.val = val
        root.left = None
        root.right = None
        root.parent = parent
        return
        
    if val < root.val:
        insertTree(root.left,val,root)
    else:
        insertTree(root.right,val,root)
```

Time: Allocating the node and linking it in to the tree is a constant-time operation after the search has been performed in O(h) time





#### Deletion from a Tree

Removing a node means linking its two descendant subtrees back into the tree  somewhere else. Three cases are listed:

* Delete node with 0 children: Simply clear the pointer to the given node
* Delete node with 1 child: Directly linked the grandchild to the parent
* Delete node with 2 children: 
  * Relabel this node with the key of its immediate successor in sorted order, which is the smallest value in the right subtree, specifically the leftmost descendant in the right subtree
  * This reduces to the problem of removing a node with at most one child

Time: O(h), require at most two search operations and constant time of pointer manipulation





### 3.2 How good are binary search trees?



* All 3 dictionary operations(search,insert,delete) take O(h) time, where h is the height of the tree. When tree is perfectly balanced, h=log n
* The data structure has no control over the order of insertion. If we insert element in sorted order, then we will produce a linear height tree where only right pointers are used
* The average height of the tree(n! possible shapes) is O(log n), which is an important example of the power of randomization. Simple algorithm offers good performance with high probability.





### 3.3 Balanced Search Trees

* Adjust the tree a little after each iteration, keeping it close enough to be balanced so the maximum height is logarithmic. Therefore, the dictionary operations take O(log n) time each

* Example: red-black trees and splay trees



**Exploiting Balanced Search Trees**

You are given the task of reading n numbers and then printing them out in sorted order. Suppose you have access to a balanced dictionary data structure, which support the 7 operations search, insert, delete, minimum, maximum, successor and predecessor in O(log n )time.



* How can you sort in O(nlogn) time using only insert and in-order traversal?

Initialize the tree using insertion and in-order traverse the tree

```python
def sort1(data):
    root = ListNode()
    while data:
        insertTree(root,data)   # each take logn time, n nodes in total
    Traverse(t)         # in-order
```



* How can you sort in O(nlogn) time using only minimum, successor and insert?

Initialize the tree using insertion, find the minimum and continuously find its successor

```python
def sort2(data):
    root = ListNode()
    while data:
        insertTree(root,data)   # each take logn time, n nodes in total
    y = findMinimum(root)
    while y:
        print(y.val)
        findSuccessor(root,y)
```



* How can you sort in O(nlogn) time using only minimum, insert, delete and search?

Initialize the tree using insertion, find the minimum, delete it and find the minimum again

```python
def sort3(data):
    root = ListNode()
    while data:
        insertTree(root,data)   # each take logn time, n nodes in total
    y = findMinimum(root)
    while y:
        print(y.val)
        delete(root,y)
        y = findMinimum(root)
```





Each of these algorithms does a linear number of logarithmic-time operations, and hence runs in O(nlogn) time. The key to exploiting balanced binary search trees is using them as black boxes.