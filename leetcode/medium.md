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
`cmd-shift-v` to preview
