# Chapter 4 - Sorting and Searching - 4 - Binary Search



## 1. Definition 

* A fast algorithm for searching in a sorted array of keys
* Locate the key in a total of logn comparisons



## 2. Implementation

```python
def binary_search(s,key,low,high):
    if low > high:
        return -1
    middle = (low + high) / 2
    
    if s[middle] == key:
        return middle
    elif s[middle] > key:
        return binary_search(s,key,low,middle-1)
    else:
        return binary_search(s,key,middle+1,high)
```





## 3. Variants: Counting Occurrences

Count the number of times a given key k occurs in a given sorted array

* Sorting groups all the copies of k into a contiguous block, we just need to find the right block anf measure its size
* Method 1: Binary search for the key k
  * Identifty the boundaries of the block by sequentially test elements to the left of k and right of k
  * Runtime: O(lgn+s)
  * This can be as bag as linear if entire array consists of the identical keys
* Method 2: Binary search for the boundary of the block containing k
  * Delete the equality test
  * Return index low instead of -1 on each unsuccessful search
  * All searches will be unsuccessful now
  * The search will proceed to the right half whenever the key is compared to an identical array element and eventually terminating at the right boundary
  * Repeat the search after reversing the direction of binary comparison to find the left boundary
  * Runtime: O(logn)



```python
def binary_search(s,key,low,high):
    if low > high:
        return low  #####
    middle = (low + high) / 2
    

    if s[middle] > key:
        return binary_search(s,key,low,middle-1)
    else:
        return binary_search(s,key,middle+1,high)
```





## 4. Square and Other Roots

Number $r$ such that $r^2=n$

* $l=1,r=n,m=(l+r)/2$
* Compare $m^2$ with $n$
* If $n\geq m^2$, the square root is larger than $m$, otherwise it's smaller than $m$
* Runtime: O(log n)



Root of function: $x$ is a root if $f(x)=0$

- $l=1,r=n,m=(l+r)/2$, $f(l)<0$, $f(r)>0$
- Compare $f(m)$ with 0
- Runtime: O(log n)







## 5. Take-Home Lesson

Binary search and its varaints are the quintessential divide-and-conquer algorithms





















