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

```python
class priority_queue(self):
	
    self.n = None;
    self.array = [None]*(PQ_SIZE+1)
```

### Storage

* We will store the $2^l​$ keys of the $l​$th lebel of a complete binary tree from left-to-right in positions $2^{l-1}​$ to $2^l-1​$
* If the key is located at position $k$, then the position of 
  * left child: $2k$
  * right child: $2k+1$
  * parent: $int(k/2)$

```python
def pq_parent(n):
	if n == 1:
        return -1
   else:
    return int(n/2)

def pd_young_child(n):
    return (2*n)
```





### Drawback

* Space efficiency: Suppose height $h$ tree is sparse, $n<2^h$, all missing internal nodes still take up space in our structure, since we must represent a full binary tree to maintain the positional mapping between parents and children
* Space efficiency requires no holes in the tree-i.e., taht each level be packed as much as it can be. Only the last level may be incomplete.
* height of the tree: $h = int(log(n))$
* This representation saves memory but it's less flexible than pointers. We cannot store arbitrary tree topologies without wasting large amount of space. We cannot move subtrees around by just changing a single pointer



### Stop and think

Problem: How can we efficiently search for a particular key in a heap?

Ans: We can't. Heap is not a binary search tree. We have to do a linear search through them.





## 2. Constructing Heaps

Heaps can be constructed incrementally, by inserting each new element into the left-most open spot in the array. namely the (n+1)st positions of a previously n-element heap.

* Ensure the desired balanced shape of the heap-labeled tree, but does not necessarily maintain the dominance ordering of the keys.
* The new key might be less than its parent in a min-heap or greater than its parent in a max-heap.
* The solution is to swap any such dissatisfied element with its parent



```python
def pq_insert(queue,x):
    # q:priority queue
    # x: element inserted
    if queue.n >=  PQ_SIZE:
        print("Warningig: priority queue overflow insert x=%d\n",x)
    else:
        queue.n += 1
        queue.q[queue.n] = x
        bubble_up(queue,queue.n)
        
def bubble_up(queue,p):
    # at root of heap, no parent
    if pd_parent(p) == -1:
        return
    # suppose we are workign on a min-heap
   	if quque.q[pq_parent(p)] > quque.q[p]:
        # constant time to swap element in an array
        pq_swap(queue,p,pq_parent(p))
        bubble_up(queue,pq_parent(p))
```



* Each swap process takes constant time at each lebel
* Height of n-element heap is log n, each insertion takes at most O(log n) time
* An initial heap of n elements can be constructed in O(n log n) time through n insertions



```python
def pd_init(queue):
    queue.n = 0
def make_heap(queue,s,n):
    #s : keys array
    #n: number of elements to be inserted
    pq_init(queue);
    for i in range(n):
        pq_insert(queue,s[i])
```





## 3. Extracting the Minimum

### Identification and Removing the top element

* Find the smallest element: easy, since the top of the heap sits in the first position of the array

```python
def extract_min(queue):
    min = -1
    if queue.n <= 0:
        print("Warning: Empty priority queue.")
    else:
        min = queue.q[1]
        queue.q[1] = queue.q[n] # swap the first and the last element
        queue.n -= 1
        bubble_down(queue,1)
    return min

def bubble_dowm(queue,p):
    # p: the index to be bubbled down
    c = pq_young_child(p)  # child index
    min_index = p
    
    for i in range(1):
        # i = 0,1 left child and right child
        # avoid overflow
        if c+i <= queue.n:
            # if the root(supposed smallest key) is larger than its two children
            # then the min index is updated by the children's index
            if queue.q[min_index] > queue.q[c+i]:
                min_index = c+i
        if min_index != p:
            # if the min_index is from children, swap the root and the children
            pq_swap(queue,p,min_index)
            # repeat this process
            bubble_down(queue,min_index)
```



* We will reach a leaf after **log n** bubble_down steps, each constant time. So root deletion is in O(log n) time
* **Heapify**: exchanging the maximum element with the last element
* **Heap sort**: Repeated heapifying process gives an O(n log n) sorting algorithm

```python
def heapsort(s,n):
    # s: array to be sorted 
    # n: number of items
    queue = priority_queue()
    make_heap(queue,s,n)
    for i in range(n):
        s[i] = extract_min(queue)
```

* It runs in worst-case O(n log n), which is the best that can be expected from any sorting algorithm
* It is an **inplace** sort, meaning it uses no extra memory.



In this scenario, heap can be constructed on n elements by incremental insertion in O(n log n) time



## 4.Faster Heap Construction

* Heap can also be constructed even faster by using the bubble_down procedure.

* Suppose we pack n keys destined for our heap into the first n elements of the priority-queue array, the shape is right but order is messed up. We can use **bubble_down** to restore this



```python
def make_heap(queue,s,n):
    queue.n = n
    # pack the array into the heap
    for i in range(n):
        queue.q[i+1] = s[i]
    # n/2 will be trivial since the last n/2 elements are leaves and don't have children
    for i in range(queue.n,0,-1):
        bubble_down(queue,i)
```

* Bubble_down takes log n time and we will perform this n times. So the upper bound of the running time is O(n log n). 
* But the height for each bubble down is different, the lower the height, the less time you take. So the total cost is actually

$$
\sum_{h=0}^{[log n]}[n/2^{h+1}]h\leq n\sum_{h=0}^{[log n]}[h/2^{h+1}]\leq 2n
$$

Note: n/2 nodes have height 0, n/4 nodes have height 1, ... , $[n/2^{h+1}]$ nodes have height h



Though we can build heap in linear time, the construction time did not dominate the complexity of heapsort. We still need O(n log n) time for the n times extract_min process.





#### Stop and think

Problem: Given an array-based heap on n elements and a real number x, efficiently determine whether the k th smallest element in the heap is greater than or equal to x? It should be O(k) in the worst-case, independent of the size of the heap. 



**Solution 1**

* Extract-min k times and test whether all of these are less than x. This overshoots the problem because it sorts the first k elements explicitly. 
* Time: O(k log n) , k times bubble_down



#### Solution 2

* The k th smallest element cannot be deeper than the k th level of the heap, since the path from k level to root  goes through in decreasing order.
* We can look at all elements on the first k levels and count how many of them are less than x
* We stop when we either find k of them(<= x ) or run out of elements(> x)
* Time: O(min(n,$2^k$)) since top k elements have $2^k$ elements



#### Solution 3 O(k)

* Look at only k elements smaller than x, plus at most O(k) elements greater than x
* Recursive solution

```python
def heap_compare(queue,i,count,x):
    # count <= 0: invalid search
    # i > n: overflow
    if count <= 0 or i > queue.n:
        return count
   	if queue.q[i] < x:
        # search left children
        count = heap_compare(queue,pq_young_child(i),count-1,x)
        # search right children, use the previous count(accumulate)
        count = heap_compare(queue,pq_young_child(i)+1,count,x)
    return count
heap_compare(queue,1,k,x) # i:start from root  k: count
```

* We search the children of all nodes of weight smaller than x until
  * We find k of them, when it return 0
  * They are exhausted, return a value larger than 0. Thus it will find enough small elements if they exist.
* Time: O(k). We only look at k elements and each for two children, so we visit 3k nodes







## 5. Sorting by Incremental Insertion

Incremental insertion technique: We build up a complicated structure on n items by first building it on n-1 items and the make the necessary changes to add the last item.