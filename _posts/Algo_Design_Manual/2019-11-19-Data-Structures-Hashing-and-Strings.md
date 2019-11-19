# Chapter 3 - Data Structures - 5 - Hashing and Strings

Source: [Algorithm Design Manual(Skiena)](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf)



### Def

* Hash table: very practical way to maintain a dictionary. We use the value of our hash function as an index into an array, and store our item at that position
* Hash function: map each key to a big integer
  * <img src="https://latex.codecogs.com/gif.latex?\alpha" /> : size of the alphabet
  * char(c): A function that maps each symbol of the alphabet to a unique integer from 0 to <img src="https://latex.codecogs.com/gif.latex?\alpha-1" />
  * <img src="https://latex.codecogs.com/gif.latex?H(S)=\sum_{i=0}^{|S|-1}\alpha^{|S|-(i+1)}\times char(s_i)" /> maps each string  to a unique integer by creating the characters of the strings as digits in a base-<img src="https://latex.codecogs.com/gif.latex?\alpha" /> number system
  * To reduce the size of the table(<img src="https://latex.codecogs.com/gif.latex?m" />), we need to take <img src="https://latex.codecogs.com/gif.latex?H(S)\,mod\, m" />





### 1. Collision Resolution

Two distinct keys will occasionally hash to the same value.

* **Chaining**: Represent the hash table as an array of m linked lists. The <img src="https://latex.codecogs.com/gif.latex?i" />th list will contain all the items that hash to the value of <img src="https://latex.codecogs.com/gif.latex?i" />. Then search, insertion and deletion reduce to the corresponding problem in linked list. But this method devotes a considerable amount of memory to pointers, which originally can be used to make the table larger
  * Traversing all the elements takes O(n+m) time because we have to scan all m buckets looking for elements



Using chaining with doubly-linked list to resolve collisions in an m-element hash table, the dictionary for n items can be implemented in the following time:

|                  | Hash table(expected) | Hash table(worst case) |
| :--------------: | :------------------: | :--------------------: |
|   Search(L,k)    |        O(n/m)        |          O(n)          |
|   Insert(L,x)    |         O(1)         |          O(1)          |
|   Delete(L,x)    |         O(1)         |          O(1)          |
|  Successor(L,x)  |        O(n+m)        |         O(n+m)         |
| Predecessor(L,x) |        O(n+m)        |         O(n+m)         |
|    Minimum(L)    |        O(n+m)        |         O(n+m)         |
|    Maximum(L)    |        O(n+m)        |         O(n+m)         |





* **Open addressing**: The hash table is maintained as an array of elements(not buckets), each initialized to null.
  * Insertion: check whether the desired position is empty. If not, find the next open spot in the table and insert it(sequential probing)
  * Searching: Going to the appropriate hash values and check to see if the item is the one we want. If not, we need to check through the length of the run
  * Deletion: Reinsert all the items in the run following the new hole
  * Traversal: O(m) since n must be at most m



* Chaining and open addressing both require O(m) to initialize an m-element hash table to null elements prior to the first insertion

* Pragmatically, a hash table is often the best data structure to maintain a dictionary



### 2. Efficient String Matching via Hashing

* Data structure: array of characters



#### Operation

* Substring search

Problem: Substring pattern matching

Input: A text string <img src="https://latex.codecogs.com/gif.latex?t" /> and a pattern string <img src="https://latex.codecogs.com/gif.latex?p" />

Output: Does <img src="https://latex.codecogs.com/gif.latex?t" /> contain the pattern <img src="https://latex.codecogs.com/gif.latex?p" /> as a substring, and if so where?



**Simple Algorithm**

* Search for the presence of pattern string <img src="https://latex.codecogs.com/gif.latex?p" /> in text <img src="https://latex.codecogs.com/gif.latex?t" /> overlays the pattern string at every position in the text, and checks whether the pattern matches the text
* Runtime: <img src="https://latex.codecogs.com/gif.latex?O(nm)" />, <img src="https://latex.codecogs.com/gif.latex?n=|t|,m=|p|" />



**Rabin-Karp Algorithm**

* Run time: linear expected-time
* Idea: Compute a given hash function on both the pattern string <img src="https://latex.codecogs.com/gif.latex?p" /> and the <img src="https://latex.codecogs.com/gif.latex?m"/>-character substring starting from the <img src="https://latex.codecogs.com/gif.latex?i"/>th position of <img src="https://latex.codecogs.com/gif.latex?t" />
* If the two strings are identical, their hash values must be the same. If not, their hash values are almost different. If somehow they are the same, we just need <img src="https://latex.codecogs.com/gif.latex?O(m)" /> time to compare the strings.
* So we have <img src="https://latex.codecogs.com/gif.latex?n-m+2" /> hash values computations( (<img src="https://latex.codecogs.com/gif.latex?n-m+1" />) for text and 1 for <img src="https://latex.codecogs.com/gif.latex?p" />), each computation takes <img src="https://latex.codecogs.com/gif.latex?O(m)" /> time, plus a small number of verification with <img src="https://latex.codecogs.com/gif.latex?O(m)" /> time, the runtime should be <img src="https://latex.codecogs.com/gif.latex?O(nm)" />

But looking at the hash function starting from the <img src="https://latex.codecogs.com/gif.latex?j"/>th position of string:

<img src="https://latex.codecogs.com/gif.latex?H(S,j)=\sum_{i=0}^{m-1}\alpha^{m-(i+1)}\times char(s_{i+j})" />

Then,

<img src="https://latex.codecogs.com/gif.latex?H(S,j+1)=\alpha(H(S,j)-\alpha^{m-1}char(s_j))+char(s_{j+m})"/>

This can be done in constant time. So the time to compute the hash values is actually $O(n+m)$ as long as we don't get frequent collision(remember the verifications takes <img src="https://latex.codecogs.com/gif.latex?O(m)" /> time ).



If the values of hash function ranges from 0 to<img src="https://latex.codecogs.com/gif.latex?M" />, then the probability of a false collision should be <img src="https://latex.codecogs.com/gif.latex?1/M" />.





### 3. Duplicate Detection Via  Hashing

* Is a given document different from all the rest in a large corpus?
  * Hash the new document into an integer, compare it to the hash codes of the rest of the corpus. Only when there is a collision is this a possible collision
* Is part of this document plagiarized from a document in a large corpus?
  * Adding, deleting and changing even one character of the document will change the hash code
  * Build a hash table of all overlapping windows(substrings) of length <img src="https://latex.codecogs.com/gif.latex?w" /> in all the documents in the corpus. Whenever their is match of hash codes, there is likely a common substrings of length <img src="https://latex.codecogs.com/gif.latex?w" /> between these two documents
  * Downside: size of table becomes as large as the documents. But we can retain a small but well-chosen subset of these hash codes for each document
* How can I convince you that  a file isn't change?
  * Check the hash codes of the documents to see whether it changes or not



### 4. Specialized Data Structures

Details are in Chapter 12.

* String data structures - Suffix trees/arrays
* Geometric data structures - kd trees
* Graph data structures - Adjacency matrix and adjacency list
* Set data structures - bit vectors, union-find data structures





