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

# Heap
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

