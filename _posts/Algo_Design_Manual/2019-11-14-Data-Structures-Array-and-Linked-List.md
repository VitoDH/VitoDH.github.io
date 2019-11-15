# Chapter 3 - Data Structures - 1 - Array and Linked List

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)

Mainly focus on the following three fundamental abstract data types

* Containers
* Dictionaries
* Priority queues

using arrays and lists



## Classification

* **Contiguously-allocated structures**: **composed of single slabs of memory**, eg, arrays, matrices, heaps and hash tables
* **Linked data structures**: **composed of distinct chunks of memory bound together by pointers**, eg, lists, trees, and graph adjacency lists.





### 1. Arrays

Structures of fixed-size data records such that each element can be efficiently located by its index or address.



#### Advantages

* Constant-time access given the index: maps directly to a particular memory address
* Space efficiency: consist purely of data
* Memory locality: easy for iterating, help exploit the high-speed cache memory



#### Disadvantages

* Cannot adjust the size in the middle of a a program's execution

* To fix the above problem, we can use dynamic array, which doubles the size of array whenever it hits the limitation(1,2,4,8,16,etc...). The time for copying the elements from the old is $O(n)$.





### 2. Pointers and Linked Structures

Pointers are the connections that hold the pieces of linked structures together. They represent the  address of a location in memory.

In C,

```C
// the item that is pointed to by pointer p
*p
// the address/pointer of a particular variable x
&x
```



#### Linked list type declaration

```C
typedef struct list{
    item_type item;
    struct list * next;   //pointer to successor
}list;
```

Note: Much of the space sued in linked data structures has to be devoted to pointers, not data.





**Operations** of linked list are as follows:



#### 2.1 Searching a List

Searching for item x in a linked list can be done iteratively or recursively.

```python
# recursive approach
def search_list(list_,x):
    if list_ is None:
        return None
    
    if list_.val == x:
        return list_
    else:
        return search_list(list_.next,x)
   

# iterative approach
def search_list(list_,x):
    while list_:
        if list_.val == x:
            return list_
        else:
            list_ = list_.next
    return None
```



#### 2.2 Insertion into a List

Since have no need to maintain the list in any particular order, we might as well insert each new item in the simplest place. Insertion at the beginning of the list avoids any need to traverse the list.

```python
def insert_list(head,x):
    p = ListNode()
    
    p.val = x
    p.next = head
    head = p
    return head
```



#### 2.3 Deletion From a List

A little complicated. We must find a pointer to the predecessor of the item to be deleted. We do this recursively,

```python
def findPredecessor(head,x):
    if head is None or head.next is None:
        return None
    
    if head.next.val == x:
        return head
    else:
        return findPredecessor(head.next,x)
```



Then we can implement the delete operation. Special care must be taken to reset the pointer to the head of the list when the first element is deleted:

```python
def delete_list(head,x):
    cur = ListNode()
    pred = ListNode()
    
    cur = search_list(head,x)
    if cur:
        pred = findPredecessor(cur)
        
        if pred is None:  # we want to delete the head
            head = cur.next
        else:
            pred.next = cur.next
    return head
```



### 3. Comparison



#### 3.1 Advantage of linked list over static array:

* Overflow on linked structures can never occur unless the memory is actually full
* Insertions and deletions are simpler than for contiguous (array) lists
* With large records, moving pointers is easier and faster than moving the items themselves



#### 3.2 Advantages of arrays:

* Linked structures require extra space for storing pointer fields
* Linked lists do not allow efficient random access to items
* Arrays allow better memory locality and cache performance than random pointer jumping





### 4. Final Thoughts

* Lists: Chopping the first element of a linked list leaves a smaller linked list. Lists are recursive objects.
* Arrays: Splitting the first k elements off of an n element array gives two smaller arrays, of size k and n-k, respectively. Arrays are recursive objects.



These lead to simple list processing and efficient divide-and-conquer algorithms such as quick sort and binary search.

