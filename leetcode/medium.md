# Add Two Numbers
### Problem
Given two non-empty linked lists representing two non-negative integers, with each digit stored in a node in *reverse order*, compute the sum of the two numbers as a linked list in reverse order.
### Example
```
l1 = [2, 4, 3]
l2 = [5, 6, 4]
returns [7, 0, 8]
because 342 + 465 = 807
```
### Solution
**Data structure**: linked list
##### Description
Iterate over both linked lists simultaneously to compute the sum step by step. Maintain a `carry` value, and keep track of current `l1` and `l2` nodes as well as the current sum node.
##### Code
```
var addTwoNumbers = function(l1, l2) {
    const head = new ListNode(0)
    let carry = 0
    
    let node1 = l1
    let node2 = l2
    
    let curr = head
    
    while (node1 || node2 || carry !== 0) {
        const tmpSum = (node1 ? node1.val : 0) + (node2 ? node2.val : 0) + carry
        const sum = tmpSum % 10
        curr.next = new ListNode(sum)
        carry = tmpSum >= 10 ? 1 : 0
        curr = curr.next
        node1 = node1 ? node1.next : undefined
        node2 = node2 ? node2.next : undefined
    }
    
    return head.next
}
```
##### Time complexity: O(max(n,m))
One pass through the longer of the two linked lists
##### Space complexity: O(max(n,m))
Maintain a sum which is at most `max(n,m) + 1`

---

# Longest Substring Without Repeating Characters
### Problem
Given a string `s`, find the length of the longest substring without repeating characters.
### Example
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```
### Solution
**Data structure**: set
##### Description
We can use the *sliding window* technique to find the longest substring in a single for loop. We initialize a set of previously seen characters and a `left` and `right` window boundary. We iterate through, and when the right boundary encounters a character in the set, we move the left boundary and remove characters from the set until we have all unique characters again, keeping track of the longest substring as we go.
##### Code
```
var lengthOfLongestSubstring = function(s) {
    let longest = 0
    let chars = new Set() // ensure no duplicate chars
    // create sliding window, iterate through string
    let left = 0
    for (let right = 0; right < s.length; right += 1) {
        const rChar = s[right]
        while (chars.has(rChar)) {
            const lChar = s[left]
            chars.delete(lChar)
            left += 1
        }
        chars.add(rChar)
        if ((right - left + 1) > longest) longest = right - left + 1
    }
    return longest
}
```
##### Time complexity: O(n)
At most, each character can be visited once by each `left` and `right` for a complexity of O(2n).
##### Space complexity: O(n)
In the worst case, the entire string has unique characters and our set contains the whole string.

---

# Longest Palindromic Substring
### Problem
Given a string `s`, return the longest palindromic substring in `s`.
### Example
```
Input: "cbbd"
Output: "bb"
```
### Solution
**Data structure**: none
##### Description
The brute force solution involves nested for loops, which gives a time complexity of O(n^3). This can be reduced by checking each character in `s` and understanding that if `s[i-1] === s[i+1]`, then `s[i-1:i+1]` is also a palindrome. We can expand outwards at each step, performing these checks until we reach a case where `s[i-1] !== s[i+1]`, at which point we can compare the length to the running max.
##### Code
```node
var longestPalindrome = function(s) {
    // store indices for longest substr
    let start = 0
    let end = 0
    
    const expandAroundCenter = (left, right) => {
        while (left >= 0 && right < s.length && s[left] === s[right]) {
            left -= 1
            right += 1
        }
        return right - left - 1
    }
    
    for (let i = 0; i < s.length; i += 1) {
        // two cases: racecar (start at e) and aabbaa (start at bb)
        const l1 = expandAroundCenter(i, i)
        const l2 = expandAroundCenter(i, i + 1)
        const len = Math.max(l1, l2)
        if (len > (end - start)) {
            start = i - Math.floor((len - 1) / 2)
            end = i + Math.floor(len / 2)
        }
    }
    
    return s.slice(start, end + 1)
}
```
##### Time complexity: O(n^2)
Iterating through is O(n), and expanding outwards at each iteration is O(n).

##### Space complexity: O(1)
Only strings are used to store data.

---

# Container With The Most Water
### Problem
Given `n` non-negative integers `a1, a2, ..., an`, where each represents a point at coordinate `(i, ai)`. `n` vertical lines are drawn such that the two endpoints of the line `i` is at `(i, ai)` and `(i, 0)`. Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.
### Example
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```
### Solution
**Data structure**: none
##### Description
The brute force solution would be to use nested for loops to check every possible volume, which would result in an O(n^2) time complexity. However, we can instead use two pointers, each starting at either end, and move them inwards on each iteration inside of a single loop. Since the distance between the two pointers is getting smaller, the only way to overcome this loss in volume would be for the shorter height to become taller. We can then move the shorter pointer over each time to see if we get a larger volume, until they meet in the middle.
##### Code
```node
var maxArea = function(height) {
    let i = 0
    let j = height.length - 1
    let largest = 0
    while (i !== j) {
        const volume = Math.min(height[i], height[j]) * (j - i)
        if (volume > largest) largest = volume
        // move the pointer of the shorter height closer
        if (height[i] < height[j]) i += 1
        else j -= 1
    }
    return largest
}
```
##### Time complexity: O(n)
Only a single pass through the array
##### Space complexity: O(1)
---

`cmd-shift-v` to preview
