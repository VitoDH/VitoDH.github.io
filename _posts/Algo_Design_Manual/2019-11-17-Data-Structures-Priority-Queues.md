# Chapter 3 - Data Structures - 4 - Priority Queues

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)



### Def

* Data structures that provide more flexibility than simple sorting, because they allow new elements to enter a system at arbitrary intervals
* Much more cost-effective to insert a new job into a priority queue





### Operations

* Insert(Q,x): Given an item x with key k, insert it into the priority queue Q
* Find-Minimum(Q)/Find-Maximum(Q): Return a pointer to the item whose key value is smaller(larger) than any other key in the priority queue
* Delete-Minimum(Q) or Delete-Maximum(Q): Remove the item from the priority queue Q whose key is minimum(maximum)





### Stop and think

What is the worst-case time complexity of the three basic priority queue operations when the basic data structure is 

* An unsorted array
  * Insert: O(1)
  * Find-minimum: actually O(n), but we can store a pointer to minimum and maintain it when inserting new values
  * Delete-minimum: O(n) (First find minimum by O(1), delete by O(1),search the new minimum by O(n))
* A sorted array:
  * Insert: O(n)	
  * Find-minimum: O(1), the first element
  * Delete-minimum: Delete takes O(n), but delete minimum or maximum only takes O(1) (We can store the array reverse order. Then delete the minimum just by decreasing the last index by 1)
* A balance binary search tree
  * Insert: O(log n), depth of the tree
  * Find-minimum: O(1), we have a pointer
  * Delete-minimum: O(log n)





|                   | Unsorted Array | Sorted Array | Balanced Tree |
| :---------------: | :------------: | :----------: | :-----------: |
|    Insert(Q,x)    |      O(1)      |     O(n)     |   O(log n)    |
|  Find-Minimum(Q)  |      O(1)      |     O(1)     |     O(1)      |
| Delete-Minimum(Q) |      O(n)      |     O(1)     |   O(log n)    |



Note: Though we have a pointer to the minimum and finding it costs us only O(1) time. But deleting it still costs us O(n) because after deleting it we need to find a new minimum again to maintain the pointer.