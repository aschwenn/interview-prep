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
