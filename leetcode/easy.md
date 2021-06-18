# Two Sum
### Problem
Given an array of ints `nums` and an int `target`, return the indices of two values in `nums` that sum to `target`
### Solution
**Data structure**: hashmap
##### Description
Iterate over `nums` to create a hashmap of shape `Map<num, index>` for O(1) lookup. While iterating, check if the complement of the current number (`target - num`) exists in the hashmap; if so, return its value in the hashmap (its index) and the current index.
##### Time complexity: O(n)
One pass through the array is O(n). Hashmap lookups are O(1).
##### Space complexity: O(n)
There are at most `n` keys stored in the hashmap

---





---
`cmd-shift-v` to preview
