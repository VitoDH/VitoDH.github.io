# Chapter 3 - Data Structures - 2 - Container and Dictionary

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)



## Container

A data structure that permits storage and retrieval of data items independent of content

Different from dictionaries, which retrieve based on key values or content.



### 1. Stacks

* Support retrieval by last-in, first-out(LIFO) order

* Stacks are simple to implement and very efficient. They are probably the right container to use when retrieval order doesn't matter at all, such as when processing batch jobs



#### Operations

* Push(x,s): Insert item x at the top of stack s
* Pop(s): Return(and remove) the top item of stack s



Algorithmically, **LIFO tends to happen in the course of executing recursive algorithms.**



### 2. Queues

* Support retrieval in first in, first out(FIFO) order

* The container holding jobs to be processed in FIFO order to **minimize the maximum time spent waiting**
* Trickier to implement
* Appropriate for applications(like certain simulations) where the order is important



#### Operations

* Enqueue(x,q): Insert item x at the back of queue q
* Dequeue(q): Return(and remove) the front item from queue q



### 3. Conclusion

* The average waiting time will be the same for FIFO and LIFO
* Stacks and queues can be effectively implemented using either arrays or linked lists. The key issue is whether an upper bound on the size of the container is known in advance, thus permitting the use of a statistically-allocated array







## Dictionary

Permits access to data items by content



### 1. Operations

* Search(D,k): Given a search key k, return a pointer to the element in dictionary D whose key value is k, if one exists
* Insert(D,x): Given a data item x, add it to the set in the dictionary D
* Delete(D,x): Given a pointer to a given data item x in the dictionary D, remove it from D
* Max(D) or Min(D): Retrieve the item with the largest(or smallest) key from D. This enables the dictionary to **serve as a priority queue**
* Predecessor(D,k) or Successor(D,k): Retrieve the item from D whose key is immediately before (or after) k **in sorted order**. Theses enable us to iterate through the elements of the data structure



### 2. Comparing Dictionary Implementations

The following problems reveal some of the inherent trade-offs of data structure design. A given data representation may permit efficient implementation of certain operations at the cost that other operations are expensive.



#### 2.1 Using array

What are the asymptotic worst-case running times for each of the 7 fundamental dictionary operations when the data structure is implemented as:

* An unsorted array
* A sorted array



| Dictionary Operation | Unsorted Array | Sorted Array |
| :------------------: | :------------: | :----------: |
|     Search(L,k)      |      O(n)      |   O(log n)   |
|     Insert(L,x)      |      O(1)      |     O(n)     |
|     Delete(L,x)      |      O(1)      |     O(n)     |
|    Successor(L,x)    |      O(n)      |     O(1)     |
|   Predecessor(L,x)   |      O(n)      |     O(1)     |
|      Minimum(L)      |      O(n)      |     O(1)     |
|      Maximum(L)      |      O(n)      |     O(1)     |

Note: Here x is the pointer pointing to the exact value



**Explanation**

* Search
  * Unsorted Array: Testing the search key k against each element of an unsorted array. Search takes linear time in the worst case when key k is not found in A
  * Sorted Array: Using binary search
* Insertion
  * Unsorted Array: Incrementing n and then copying item x to the  $n$th cell in the array $A[n]$
  * Sorted Array: Making room for a new item may require moving many items arbitrarily
* Deletion
  * Unsorted Array: Just write over $A[x]$ with $A[n]$ and decrement n
  * Sorted Array: Filling a hoe may require moving many items arbitrarily
* Predecessor and Successor
  * Unsorted Array: Physical predecessor is not necessarily the logical predecessor. It require a sweep through all n elements of A to  determine the winner
  * Sorted Array: Predecessor and successor to A[x] are A[x-1] and A[x+1]

* Minimum and Maximum
  * Unsorted Array: Require linear sweeps to identify in an unsorted array
  * Sorted Array: Minimum A[1] and Maximum A[n]





#### 2.2 Using linked list

What are the asymptotic worst-case running times for each of the 7 fundamental dictionary operations when the data structure is implemented as:

- A singly-linked unsorted array
- A doubly-linked unsorted array
- A singly-linked sorted array
- A doubly-linked sorted array



| Dictionary Operation | Singly Unsorted | Doubly Unsorted | Singly Sorted | Doubly Sorted |
| :------------------: | :-------------: | :-------------: | :-----------: | :-----------: |
|     Search(L,k)      |      O(n)       |      O(n)       |     O(n)      |     O(n)      |
|     Insert(L,x)      |      O(1)       |      O(1)       |     O(n)      |     O(n)      |
|     Delete(L,x)      |      O(n)       |      O(1)       |     O(n)      |     O(1)      |
|    Successor(L,x)    |      O(n)       |      O(n)       |     O(1)      |     O(1)      |
|   Predecessor(L,x)   |      O(n)       |      O(n)       |     O(n)      |     O(1)      |
|      Minimum(L)      |      O(n)       |      O(n)       |     O(1)      |     O(1)      |
|      Maximum(L)      |      O(n)       |      O(n)       |     O(1)      |     O(1)      |

Note: As with unsorted arrays, search operations are slow  while maintenance operations are fast



**Explanation**

- Search
  - Sorting provides less benefits for linked list than arrays because binary search is not available.
  - It only provides quick termination of unsuccessful searches. But the worst case is still linear time
- Insertion
  - Unsorted Array: Insert at the end
  - Sorted Array: Scan through the list to find a place to insert
- Deletion
  - For singly linked list, we need a pointer to the element pointing to x in the list. But we can't access this predecessor directly and need linear time to search for it.
  - However, doubly-linked list avoid this problem
  - Sorted doubly-linked list is faster than the sorted array because we don't have to fill the hole by moving array elements
- Predecessor and Successor
  - The logical successor is equivalent to the node successor for sorted list
  - Singly list will have problem of accessing predecessor

- Minimum and Maximum

  - Minimum: if sorted, just pick the head

  - Maximum: usually require $\Theta(n)$ time to reach in either singly or doubly-linked lists. However, we can set a separate tail pointer on sorted doubly-linked list

    - insertion: check whether last->next == NULL
    - deletion: last = last.pred if last is deleted

    For sorted singly-linked list, still use a tail pointer, charge the cost at deletion by adding an extra linear sweep to update the pointer 





## Conclusion

* Data structure design must balance all the different operations it supports. The fastest data structure to support both operation A and B may not be fastest structure to support either A or B



