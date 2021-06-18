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

# Reverse Integer
### Problem
Given a signed 32-bit integer `x`, return `x` with its digits reversed. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-231, 231 - 1]`, then return 0.
### Example
```
Input: x = 123
Output: 321
```
### Solution
**Data structure**: none
##### Description
Digits can be popped off in a while loop using modulus and division. A check will need to be performed each time to ensure the newly created value won't overflow.
##### Code
```node
var reverse = function(x) {
    let result = 0
    const intLimit = Math.pow(2, 31)
    while (x !== 0) {
        const mod = x % 10
        if ((intLimit - 1 - mod) / 10 < result) return 0
        if ((0 - intLimit - mod) / 10 > result) return 0
        result = result * 10 + mod
        x = Math.trunc(x / 10)
    }
    return result
}
```
##### Time complexity: O(log(n))
Dividing by 10 each time results in a logarithmic time complexity.
##### Space complexity: O(1)

---



---
`cmd-shift-v` to preview
