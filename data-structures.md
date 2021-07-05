# HashMap
**Lookup**: O(1)
**Insertion**: O(1)
```javascript
const map = {}
map['key'] = 1
if ('key' in map) // do something
```
##### Properties
* Each key must be unique

---

# Set/Hashset
**Lookup**: O(1)
**Insertion**: O(1)
```javascript
const set = new Set()
set.add(1)
set.add(2)
set.delete(2)
console.log(set.has(1)) // true
set.clear()
```
##### Properties
* Each value must be unique

---

# Binary Tree

A binary tree is a type of tree where each node has up to two children:

```
      tree
      ----
       1    <-- root
     /   \
    2     3  
   /   
  4
```

Possible binary tree attributes:

**Full**: Each node has 0 or 2 children.

```          
             18
           /    \   
         15     20    
        /  \       
      40    50   
    /   \
   30   50
```

**Complete**: All levels are completely filled except possibly the last level.

```
              18
           /      \  
         15        30  
        /  \      /  \
      40    50  100   40
```

**Perfect**: All internal nodes have two children and all leaf nodes are at the same level (same as last diagram).

**Balanced**: The height of the tree is O(log n)

### Binary Search Tree

A binary search tree is a binary tree whose nodes have a left child less than the parent and a right child greater than the parent.

#### Balanced
**Lookup**: O(logn)
**Insertion**: O(logn)

#### Unbalanced
**Lookup**: O(n)
**Insertion**: O(n)

Because of the high unbalanced time complexity, self-balancing trees such as AVL or red-black are more effective than the basic BST.

### Heap

A heap is a type of binary tree often used for quick min/max value retrieval.

**Lookup**: O(logn)
**Insertion**: O(logn)
**Heapify**: O(n)

**Max Heap**: Type of heap where each node is greater than its children
**Min Heap**: Type of heap where each node is less than its children

```python
# importing "heapq" to implement heap queue
import heapq
  
# initializing list
li = [5, 7, 9, 1, 3]
  
# using heapify to convert list into heap
heapq.heapify(li)
  
# printing created heap
print ("The created heap is : ",end="")
print (list(li))
  
# using heappush() to push elements into heap
# pushes 4
heapq.heappush(li,4)
  
# printing modified heap
print ("The modified heap after push is : ",end="")
print (list(li))
  
# using heappop() to pop smallest element
print ("The popped and smallest element is : ",end="")
print (heapq.heappop(li))
```

> Heap is not available in JavaScript standard libraries

> A max heap can be created by negating all of the values in the array before running `heapify`, and negating any popped values

---

# Trie (prefix tree)

**Lookup**: O(M)
**Insertion**: O(M)

> (where `M` is the maximum key length)

Tries are used to quick retrieval of keys. A trie is a type of tree that stores characters and can be used to search for strings or keys by searching down the tree character by character.

```
                       root
                    /   \    \
                    t   a     b
                    |   |     |
                    h   n     y
                    |   |  \  |
                    e   s  y  e
                 /  |   |
                 i  r   w
                 |  |   |
                 r  e   e
                        |
                        r
```

```c
// Trie node 
struct TrieNode 
{ 
     struct TrieNode *children[ALPHABET_SIZE];
     // isEndOfWord is true if the node 
     // represents end of a word 
     bool isEndOfWord; 
}; 
```

https://www.geeksforgeeks.org/trie-insert-and-search/

### Compressed Trie
A compressed trie is obtained from a standard trie by joining chains of single nodes:

```
                       root
                    /   \    \
                   the  an   bye
                  / |   |  \    
                ir er  swer y
```

### Ternary Search Tree
A TST is a type of trie where the child nodes of a standard trie are ordered as a binary search tree. Each node contains 3 pointers (rather than 26): a left pointer pointing to a lesser node than the current, an equal pointer, and a right pointer pointing to a greater node.

**Pros**: more space-efficient (fewer pointers), more efficient when strings share a common prefix

https://www.geeksforgeeks.org/ternary-search-tree/

# Priority Queue

A priority queue is a type of queue that also has a priority attached to each element/node. Items with higher priority are dequeued before items of lower priority, regardless of insertion order. It has the following operations:

```
insert()
getHighestPriority()
deleteHighestPriority()
```

Priority queues are generally implemented with heaps, which provide better performance than arrays or linked lists. In a binary heap, `getHighestPriority()` can be implemented in O(1) time, `insert()` can be implemented in O(Logn) time and `deleteHighestPriority()` can also be implemented in O(log n) time.

https://www.geeksforgeeks.org/heap-and-priority-queue-using-heapq-module-in-python/

