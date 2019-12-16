# Chapter 4 - Sorting and Searching - 2 - Mergesort: Sorting by Divide-and-Conquer

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)



## 1. Recursive Approach

* Partition the elements into two groups
* Sort each of the smaller problems recursively
* Interleave the two sorted lists to totally order the elements


$$
Mergesort(A[1,n])=Merge(MergeSort(A[1,[n/2]]),MergeSort(A[n/2]+1,n))
$$


## 2. Analysis

### Efficiency

Depends on how efficiently we combine the two sorted halves into a single sorted list



### How to merge

* The smallest overall item in two lists sorted in increasing order must sit at  the top of one of the two lists
* The smallest element can be removed, leaving two sorted lists behind
* Using at most $n-1$ comparison or O(n) total work we can combine two lists into one





### Complexity

* In general, the work done on the kth level involves merging $2^k$ pairs sorted list, each of size $n/2^{k+1}$, for a total of at most $n-2^k$ comparisons
* Linear work is done merging all the elements on each level. Each of the n elements appears in exactly one subproblem on each level
* The number of elements in a subproblem gets halved at each level. So the recursion goes $log(n)$ deep
* Total run time in the worst case $O(nlogn)$



### Advantage

* Does not rely on random access to eleements as does heapsort or quicksort



### Disadvantage

* Need an auxiliary buffer when sorting arrays



## 3. Implementation

We first copy each subarray to a separate queue and merge these elements back into the array.

```python
def mergesort(s,low,high):
    if low < high:
    	middle = int((low+high)/2)
        mergesort(s,low,middle)
        mergesort(s,middle+1,high)
        merge(s,low,middle,high)

def merge(s,low,middle,high):
    buffer1 = []
    buffer2 = []
    
    for i in range(low,middle):
        buffer1.append(s[i])
    for i in range(middle+1,high):
        buffer2.append(a[i])
        
    i = low
    
    while (!(len(buffer1)==0 or len(buffer2)==0)):
        if buffer1[0] <= buffer2[0]:
            s[i] = buffer1.pop(0)
        else:
            s[i] = buffer2.pop(0)
        i += 1
    while len(buffer1)!=0:
        s[i] = buffer1.pop(0)
   		i += 1
    while len(buffer2)!=0:
        s[i] = buffer2.pop(0)
        i += 1
    
```

































