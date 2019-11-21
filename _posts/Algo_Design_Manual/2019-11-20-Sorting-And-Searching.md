# Chapter 4 - Sorting and Searching - 1 - Applications and Pramatics

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)



## 1. Application of Sorting

**Highlight: ** Clever sorting algorithm exists that run in O(nlogn)



### Aplications

* Searching: Binary search test whether an item is in a dictionary in O(log n) time, provided that keys are all sorted
* Closest pair: 
  * Given a set of n numers, find the pair of numbers that have the smallest difference between them 
  * As long as the list is sorted, a linear scan completes the job
* Element uniqueness: 
  * Are there any duplicates in a give nset of n items?
  * Special case of cloest pair: whether there is a gap of 0
  * Sort the numbers, linear scan through all adjacent pairs
* Frequency distribution:
  * Given a set of n items, which element occurs the largest number of times in the set?
  * Sort the items, sweep from left to right and count them
  * Find out how often an arbitrart element k occurs
    * Look up k using binary search in a sorted array of keys
    * Walking to the left of this point until the element is not k and do the same to the right
    * Time: O(log n + c)  logn for search and c is the number of occurrences of k
    * Even better, we can use binary search to search for $k-epsilon$ and $k+epsilon$ to find the begining and ending position of k
* Selection:
  * If the keys are placed in sorted order, the kth largest can be found in O(1) time by loiking at the kth positions of the array. Similarly, the median appears in the (n/2) positions
* Convex hulls













## 2. Pragmatics of Sorting