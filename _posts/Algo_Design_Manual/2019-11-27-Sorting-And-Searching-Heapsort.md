# Chapter 4 - Sorting and Searching - 2 - Heapsort

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)



## 1. Heaps

### Definition

* Simple and elegant data structure for efficiently supporting the priority queue operations insert and extract-min
* Maintain a partial order on the set of  elements which is weaker than the sorted order but stronger than random order

* Heap-labeled tree: a binary tree such that the key labeling of each node dominates the key labeling of each of its children
  * min-heap: a node dominates its children by containing a smaller key than they do
  * max-heap: parent nodes dominate by being bigger



### Comparison with binary search tree:

* Binary search tree: the memory used by the pointers can easily outweigh the size of keys, which is the data we are really interested in
* Heap: slick data structure, enabling us to represent binary trees without using any pointers but store data as an array of keys and use the position of keys to implicitly satisfy the role of pointers



Note: Assume the array starts with index 1

```c++
typedef struct{
    item_type q[PQ_SIZE+1];
    int n;
}priority_queue;
```

### Storage

* We will store the $2^l$ keys of the $l$th lebel of a complete binary tree from left-to-right in positions $2^{l-1}$ to $2^l-1$
* If the key is located at position $k$, then the position of 
  * left child: $2k$
  * right child: $2k+1$
  * parent: $int(k/2)$



### Drawback

* Space efficiency: Suppose height $h$ tree is sparse, $n<2^h$, all missing internal nodes still take up space in our structure, since we must represent a full binary tree to maintain the positional mapping between parents and children
* Space efficiency requires no holes in the tree-i.e., taht each level be packed as much as it can be. Only the last level may be incomplete.
* height of the tree: $h = int(log(n))$
* This representation saves memory but it's less flexible than pointers. We cannot store arbitrary tree topologies without wasting large amount of space. We cannot move subtrees around by just changing a single pointer



### Stop and think

Problem: How can we efficiently search for a particular key in a heap?

Ans: We can't. Heap is not a binary search tree. We have to do a linear search through them.





## 2. Constructing Heaps







