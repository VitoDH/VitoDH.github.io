# Chapter 4 - Sorting and Searching - 2 - Quicksort: Sorting by Randomization

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)



## 1. Definition 

* Select a random item p from the n items we seek to sort. Separate the n-1 other items into two piles
* A low pile with all the elements that appear before p in sorted order
* A high pile with all the elements taht appear after p in sorted order




## 2. Advantages

* The pivot element p ends up in the exact array position it will reside in the final sorted order
* After partitioning, no element flops to the other side in the final sorted order





## 3. Implementation

```python
def quicksort(s,low,high):
    p = None # index of partitioning
    
    if (high - low) > 0:
        p = partition(s,low,high)
        quicksort(s,low,p-1)
        quicksort(s,p+1,high)

def partition(s,low,high):
    p = high # select the last element to be the pivot
    firsthigh = low
    for i in range(low,high):
        if s[i] < s[p]:
            # swap the element between firsthigh and i
            # to make sure that elements left to the firsthigh is smaller than pivot
            s[i],s[firsthigh] = s[firsthigh],s[i]
            firsthigh += 1
   # move the pivot to the firsthigh position
   s[p],s[firsthigh] = s[firsthigh],s[p]
   return firsthigh
```





### Complexity

* The partitioning step consists of at most n swaps
* Quicksort build a recursion tree of nested subranges of the n-element array
* Runtime: O(nh) where h is the height of the recursion tree
  * Lucky case: Repeatedly pick the median element as the pivot: h=logn
  * Unlucky case: The pivot always splits the array as unequally as possible, i.e., the pivot is always the biggest or the smallest element in the subarray: h=n
* Expected time: $\Theta(nlogn)$
  * good enough points: n/4 to 3/4n, height of good enough points $h_g=log_{\frac{4}{3}}n$
  * $h\approx 2h_g$





## 4. Randomized Algorithm

* Add an initial step to the algorithm
  * We randomly permute the order of the n elements before we try to sort them, which takes O(n) time
  * Provide the guarantee that we can expect $\Theta(nlogn)$ running time whatever the initial input is
* Randomization
  * Improve algorithms with bad worst-case but good average-case complexity
  * Make algorithms more robust to boundary cases and more efficient on highly structured input instances that confound heuristic decisions





## 5. Notes

* Is Quicksort really quick?
  * The experiments shows that the quicksort is typically 2-3 times faster than mergesort or heapsort
  * The primary reason is that the operations in the innermost loop are simpler

























