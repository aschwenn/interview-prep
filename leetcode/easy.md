# Two Sum
### Problem
Given an array of ints `nums` and an int `target`, return the indices of two values in `nums` that sum to `target`
### Solution
**Data structure**: hashmap
##### Description
Iterate over `nums` to create a hashmap of shape `Map<num, index>` for O(1) lookup. While iterating, check if the complement of the current number (`target - num`) exists in the hashmap; if so, return its value in the hashmap (its index) and the current index.
##### Code
```javascript
var twoSum = function(nums, target) {
    // constructing lookup table of (num, index)
    const dict = {}
    for (let i = 0; i < nums.length; i += 1) {
        const complement = target - nums[i]
        if (complement in dict) return [dict[complement], i]
        else dict[nums[i]] = i
    }
}
```
##### Time complexity: O(n)
One pass through the array is O(n). Hashmap lookups are O(1).
##### Space complexity: O(n)
There are at most `n` keys stored in the hashmap

### Part two: array is sorted in ascending order

We can instead optimize for this scenario by maintaining two pointers at each end of the array. We can sum the values at each pointer to see if they match the target, and move the left if our sum is too low or the right if our sum is too high.

##### Code

```javascript
var twoSum = function(numbers, target) {
    let left = 0
    let right = numbers.length - 1
    while (left < right) {
        const sum = numbers[left] + numbers[right]
        if (sum === target) return [left + 1, right + 1]
        else if (sum > target) right -= 1
        else left += 1
    }
}
```

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
```javascript
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

# Palindrome Number
### Problem
Given an integer x, return true if x is palindrome integer. Avoid converting the integer into a string for the solution.
### Example
```
Input: x = 121
Output: true

Input: x = -121
Output: false
```
### Solution
**Data structure**: array
##### Description
Use a while loop to break the integer into its digits and store it in an array. Then, compare the latter half of the array to the former half.
##### Code
```javascript
var isPalindrome = function(x) {
    if (x < 0) return false
    const arr = []
    // O(logn)
    while (x > 0) {
        arr.push(x % 10)
        x = Math.trunc(x / 10)
    }
    // O(1/2 log(n))
    for (let i = 0; i <= arr.length / 2; i += 1) {
        if (arr[i] !== arr[arr.length - 1 - i]) return false
    }
    return true
}
```
##### Time complexity: O(logn)
The while loop takes O(logn), and the for loop afterwards takes O(1/2 logn).
##### Space complexity: O(n)
The array must hold each digit.

---

# Roman to Integer
### Problem
Given a roman numeral, convert it to an integer.
### Example
```
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```
### Solution
**Data structure**: hashmap
##### Description
We can use a hashmap to convert the Roman numerals to integers with an O(1) lookup time. Then, we can iterate through each numeral, checking ahead in case of a 4, 9, 40, etc.
##### Code
```javascript
var romanToInt = function(s) {
    const dict = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    }
    let sum = 0
    for (let i = 0; i < s.length; i += 1) {
        if (i < s.length - 1 && /(I[VX])|(X[LC])|(C[DM])/.test(s.slice(i, i + 2))) {
            sum += dict[s[i + 1]] - dict[s[i]]
            i += 1
        } else {
            sum += dict[s[i]]
        }
    }
    return sum
}
```
##### Time complexity: O(n)
We iterate through the string one time.
##### Space complexity: O(1)

---

# Valid Palindrome
### Problem
Given a string `s`, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
### Example
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```
### Solution
**Data structure**: two pointers
##### Description
We can use two pointers at either end of the string to check at each index that the string is symmetrical/palindromic.
##### Code
```javascript
var isPalindrome = function(s) {
    s = s.toLowerCase().replace(/[^a-z0-9]/g, '')
    let left = 0
    let right = s.length - 1
    while (left < right) {
        if (s[left] !== s[right]) return false
        left += 1
        right -= 1
    }
    return true
}
```
##### Time complexity: O(n)
The cleanup/regex call is O(n), and the while loop is O(1/2 n).
##### Space complexity: O(1)
The original string is modified, so no extra memory is used beyond the pointers.

---

# Valid Parenthesis
### Problem
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
### Example
```
Input: s = "()[]{}"
Output: true
```
### Solution
**Data structure**: stack, hashmap
##### Description
Iterate through `s`, pushing each opening bracket onto the stack. If a closing bracket is encountered, pop from the stack and compare the result to the closing bracket for a match. After iterating through `s`, the stack should be empty if the string is valid.
##### Code
```javascript
var isValid = function(s) {
    const arr = []
    const map = { '{': '}', '(': ')', '[': ']' }
    for (let i = 0; i < s.length; i += 1) {
        const char = s.charAt(i)
        if (char in map) arr.push(char)
        else {
            const open = arr.pop()
            if (map[open] !== char) return false
        }
    }
    return !arr.length
}
```
##### Time complexity: O(n)
We make a single pass through `s`.
##### Space complexity: O(n)
We could store up to O(1/2 n) open brackets in the stack.

---

# Reverse a Linked List
### Problem
Given the head of a singly linked list, reverse the list, and return the reversed list.
### Example
```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```
### Solution
**Data structure**: none (in-place)
##### Description
Iterate through the linked list, pointing `next` to the previous node and storing the current and previous nodes.
##### Code
```javascript
var reverseList = function(head) {
    if (!head) return head
    
    let prev = null
    let curr = head
    
    while (curr) {
        const next = curr.next
        curr.next = prev
        prev = curr
        curr = next
    }
    
    return prev
    
}
```
##### Time complexity: O(n)
##### Space complexity: O(1)

---

# Invert Binary Tree
### Problem
Given the root of a binary tree, invert the tree, and return its root.
### Example
```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```
### Solution
**Data structure**: none (in-place)
##### Description
Use a recursive approach to switch the two children each time.
##### Code
```javascript
var invertTree = function(root) {
    if (!root) return null
    const tmp = root.left
    root.left = invertTree(root.right)
    root.right = invertTree(tmp)
    return root
}
```
##### Time complexity: O(n)
We have to at least visit each node to invert it.
##### Space complexity: O(1)

---

# Climbing Stairs
### Problem
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
### Example
```
Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step
```
### Solution 1
**Data structure**: array
##### Description
We can write a recursive top-down solution that simulates climbing up the stairs. At each step, we can either go up one or two stairs until we reach the top. We can run a function `climb` with a step `i`, where the result is `climb(i + 1) + climb(i + 2)`. Rather than recompute these each time, we can memoize.

##### Code
```javascript
var climbStairs = function(n) {
    const memo = new Array(n)
    
    const climb = (i) => {
        if (i > n) return 0
        if (i === n) return 1
        if (memo[i]) return memo[i]
        memo[i] = climb(i + 1) + climb(i + 2)
        return memo[i]
    }
    
    return climb(0)
}
```
##### Time complexity: O(n)
##### Space complexity: O(n)

### Solution 2
**Data structure**: array
##### Description
We can write an iterative bottom-up solution that simulates climbing up the stairs in a similar way, by understanding that `dp[i] = dp[i - 1] + dp[i - 2]`.

##### Code
```javascript
var climbStairs = function(n) {
    const dp = new Array(n + 1)
    dp[1] = 1
    dp[2] = 2
    
    for (let i = 3; i <= n; i += 1) {
        dp[i] = dp[i - 1] + dp[i - 2]
    }
    
    return dp[n]
}
```
##### Time complexity: O(n)
##### Space complexity: O(n)

### Solution 3
**Data structure**: none
##### Description
We can optimize further to avoid saving each result, cutting spatial complexity down to constant time.

##### Code
```javascript
var climbStairs = function(n) {
    if (n === 1) return 1
    
    let first = 1
    let second = 2
    
    for (let i = 3; i <= n; i += 1) {
        const tmp = first + second
        first = second
        second = tmp
    }
    
    return second
}
```
##### Time complexity: O(n)
##### Space complexity: O(1)

---
# Merge Two Sorted Lists
### Problem
Merge two sorted linked lists and return it as a sorted list. The list should be made by splicing together the nodes of the first two lists.
### Example
```
Input: l1 = [1,2,4], l2 = [1,3,4]
Output: [1,1,2,3,4,4]
```
### Solution
**Data structure**: linked list
##### Description
Check through both lists simultaneously, adding the lesser node to the new linked list each time.
##### Code
```javascript
var mergeTwoLists = function(l1, l2) {
    const prehead = new ListNode()
    let current = prehead
    while (l1 && l2) {
        if (l1.val <= l2.val) {
            current.next = l1
            l1 = l1.next
        }
        else {
            current.next = l2
            l2 = l2.next
        }
        current = current.next
    }
    current.next = l1 ? l1 : l2
    return prehead.next
}
```
##### Time complexity: O(n)

##### Space complexity: O(n)
In JavaScript this is O(n), but in languages with pointers it would be constant time.

---

# Linked List Cycle
### Problem
Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.
### Example
```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```
### Solution
**Data structure**: none
##### Description
We can keep two pointers, one fast and one slow. The fast will increment by two steps each time, and the slow will increment by one. If they meet, we have a loop. If the fast pointer reaches the end of the linked list, there is no loop.
##### Code
```javascript
var hasCycle = function(head) {
    if (!head || !head.next) return false
    let slow = head
    let fast = head.next
    while (slow !== fast) {
        if (!fast || !fast.next) return false
        slow = slow.next
        fast = fast.next.next
    }
    return true
}
```
##### Time complexity: O(n)
If there is no loop, one pass through the list gives O(n). If there is a loop, we need an initial pass, plus the amount of steps required for the fast pointer to lap the slow pointer `k`, which gives us O(n + k).
##### Space complexity: O(1)

---

# Convert Sorted Array to Binary Search Tree
### Problem
Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

A height-balanced binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.
### Example
```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
```
### Solution
**Data structure**: binary search tree
##### Description
We can recursively create the binary tree from a sorted array by selecting the middle node (left middle node in this case, but it could also be the right) to be the root of a subtree, and recursively calling a function to fill out the left and right children.
##### Code
```javascript
var sortedArrayToBST = function(nums) {
    const createTree = (left, right) => {
        if (left > right) return null
        const middle = Math.floor((left + right) / 2)
        const root = new TreeNode(nums[middle])
        root.left = createTree(left, middle - 1)
        root.right = createTree(middle + 1, right)
        return root
    }
    
    return createTree(0, nums.length - 1)
}
```
##### Time complexity: O(n)

##### Space complexity: O(n)


---

# Count Primes
### Problem
Count the number of prime numbers less than a non-negative number, `n`.
### Example
```
Input: n = 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```
### Solution
**Data structure**: array
##### Description
The Sieve of Eratosthenes can be used to "remove" all integers from 2 to `n` that aren't prime, by iterating from 2 to `sqrt(n)` and marking all multiples of each as being compound numbers. To save time, we can actually mark all multiples from `p^2` to `sqrt(n)`, where `p` is the iterated integer.
##### Code
```javascript
var countPrimes = function(n) {
    if (n <= 2) return 0
    
    const arr = (new Array(n)).fill(true)
    const sqrt = Math.sqrt(n)
    for (let p = 2; p <= sqrt; p += 1) {
        if (!arr[p]) continue
        for (let j = p * p; j < n; j += p) arr[j] = false
    }
    
    return arr.slice(2).filter((v) => v).length
}
```
##### Time complexity: O(sqrt(n) loglogn)
Checking integers from `2` to `n` uses O(sqrt(n)) time, and "crossing off" multiples for each of these uses O(loglogn) time ([proof](http://www.cs.umd.edu/~gasarch/BLOGPAPERS/sump.pdf)).
##### Space complexity: O(n)


---


`cmd-shift-v` to preview
