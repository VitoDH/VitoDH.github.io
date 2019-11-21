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
* Convex hulls：
  * What is the polygon of smallest area that contains a given set of n points in two dimensions?
  * Sort the points by x-coordinate, the points can be inserted from left to right into the hull
  * Adding new right-most point may cause others to be deleted, but we can quickly identify these points because they lie inside the polygon formed by adding the new point
  * Time: O(n)





**Lesson**: Sorting  the data is one of the first things any algorithm designer should try in the quest for efficiency.



### Stop and Think

**Problem**: Give an efficient algorithm to determine whether two sets (of size m and n) are disjoint. Analyze the worst-case complexity in terms of m and n, consider the case where m<<n.



**Solution**:

* First sort the big set (size n)
  * Sort the big set in O(n logn) time
  * Binary search each element of small set to see if it exists in the big set in O(m log n) time
  * Total time: O((n+m) log n)
* First sort the small set (size m)
  * Sort the small set in O(m log m) time
  * Binary search each element of big set to see if it exists in the small set in O(n log m) time
  * Total time: O((n+m) log m
* Sort both sets
  * Sort the big set in O(n log n) and the small set in O(m log m) time
  * Compare the smallest elements of the two sorted set and discard the smaller one if they are not identical
  * Repeat step 2 until we find the identical elements or reach the end of the sets
  * Total time: O(n log n + m log m + n + m)
* Which is the fastest method?
  * Small-set sorting is faster: log m < log n when m < n
  * (n+m)log m < n log n asymptotically since n+m<2n when m < n
  * When m is in constant size, this can be done in linear time O(n)
  * Expected linear time: 
    * Use hashing: Build a hash table containing the elements of both sets, and verify that collisions in the same bucket are in fact identical elements
    * In practice, it's the best solution







## 2. Pragmatics of Sorting



* Increasing or decreasing order?
  * Different applications call for different orders
* Sorting just the key or an entire record?
  * Sorting a data set involves maintaining the integrity of complex data record.
  * Eg: a mailing list sorted by names as the key field, we need to retain the linkage between names and addresses.
* What should we do with equal keys?
  * Elements with equal key values will all bunch together in any total order, but sometimes we nned to find out their relative order
  * Resort to secondary keys and resolve ties in a meaningful way
  * **Stable**: leave the items in the same relative order as in the original permutation. Few fast algorithms are stable. It can be achieved for any sorting algorithm by adding the initial position as a secondary key
  * Certain efficient sort algorithms (eg. quick sort) may fall into quadratic time without any engineering of larger number of ties
* What about non-numerical data?
  * Alphabetizing is the sorting of text strings
  * Libraries have very complete and complicated rules concerning the relative collating sequence of characters and punctuatio
  * Specify the comparison to your sorting algorithm is with an application-specific pairwise-element comparison functin







