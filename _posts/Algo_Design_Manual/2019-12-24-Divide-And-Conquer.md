# Chapter 4 - Sorting and Searching - 5 - Divide And Conquer



## 1.Design Paradigms

### 1. Dynamic Programming

Remove one element from the problem, solve the smaller problem, and then uses the solution to this smaller problem to add back the element in the proper way



### 2. Divide and Conquer

Split the problem in halves, solve each half, then stiches the pieces back together to form a full solution 





## 2. Recurrence Relations

E.g. 

* Fibonacci numbers

$$
F_n=F_{n-1}+F_{n-2}
$$

* Polynomial

$$
a_n=a_{n-1}+1,a_1=1\rightarrow a_n=n
$$

* Exponential

$$
a_n=2a_{n-1},a_1=1\rightarrow a_n=2^{n-1}
$$

* Factorial

$$
a_n=na_{n-1},a_1=1\rightarrow a_n=n!
$$





## 3. Divide-and-Conquer Recurrences

* Break a given problem into some number of smaller pieces a, each of which is of size n/b. Further, they spend f(n) time to combine these subproblem solutions into a complete result

* T(n): The worst-case time the algorithm takes to solve a problem of size n

$$
T(n)=aT(n/b)+f(n)
$$

E.g.

* Sorting

$$
T(n)=2T(n/2)+O(n)
$$

$$
T(n)=O(nlogn)
$$

* Binary Search

$$
T(n)=T(n/2)+O(1)
$$

$$
T(n)=O(logn)
$$

* Fast Heap Construction

$$
T(n)=2T(n/2)+O(logn)
$$

$$
T(n)=O(n)
$$

* Matrix Multiplication

$$
T(n)=7T(n/2)+O(n^2)
$$

$$
T(n)=O(n^{2.81})
$$

