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
```javascript
var addTwoNumbers = function(l1, l2) {
    const head = new ListNode()
    let carry = 0
    let curr = head
    
    while (l1 || l2 || carry !== 0) {
        const tmpSum = (l1 ? l1.val : 0) + (l2 ? l2.val : 0) + carry
        curr.next = new ListNode(tmpSum % 10)
        carry = tmpSum >= 10 ? 1 : 0
        curr = curr.next
        l1 = l1 ? l1.next : null
        l2 = l2 ? l2.next : null
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
```javascript
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
```javascript
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
```javascript
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

# Integer to Roman
### Problem
Convert an integer to Roman numerals.
### Example
```
Input: num = 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```
### Solution
**Data structure**: 2D array
##### Description
We can check each digit starting with the least significant, and convert it to its corresponding Roman numeral(s). We can also use a 2D array to keep track of which Roman numerals are used for each power of 10.
##### Code
```javascript
var intToRoman = function(num) {
    let sum = []
    let i = 0
    const numerals = [['I', 'V'], ['X', 'L'], ['C', 'D'], ['M', '']]
    while (num > 0) {
        const digit = num % 10
        
        if (digit > 0 && digit < 4)
            sum.push(numerals[i][0].repeat(digit))
        else if (digit === 4)
            sum.push(`${numerals[i][0]}${numerals[i][1]}`)
        else if (digit === 5)
            sum.push(numerals[i][1])
        else if (digit > 5 && digit < 9)
            sum.push(`${numerals[i][1]}${numerals[i][0].repeat(digit - 5)}`)
        else if (digit === 9)
            sum.push(`${numerals[i][0]}${numerals[i + 1][0]}`)
    
        console.log(sum)
        
        num = Math.trunc(num / 10)
        i += 1
    }
    return sum.reverse().join('')
}
```
##### Time complexity: O(logn)
We only check each digit, which results in logn iterations.
##### Space complexity: O(1)

---

# Three Sum (3Sum)
### Problem
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.
### Example
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```
### Solution
**Data structure**: 2 pointers
##### Description
Three sum can be thought of as essentially two sum with one number held constant. We can use an outer loop to iterate through all numbers to hold one constant, and perform the two sum algorithm on the remaining. We should also sort the array initially to be able to avoid duplicates when identifying triplet sets. Since the array will be sorted, we can use the two-pointer method from Two Sum II.
##### Code
```javascript
var threeSum = function(nums) {
    const result = []
    
    const twoSum = (i) => {
        let left = i + 1
        let right = nums.length - 1
        
        while (left < right) {
            const sum = nums[left] + nums[right] + nums[i]
            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]])
                left += 1
                right -= 1
                // skip duplicates
                while (nums[left] === nums[left - 1] && left < right) left += 1
            }
            else if (sum < 0) left += 1
            else right -= 1
        }
    }
    
    // sort first to be able to apply twosum II logic
    nums.sort((a, b) => a - b)
    for (let i = 0; i < nums.length; i += 1) {
        if (nums[i] > 0) break
        // skip duplicates
        if (nums[i] === nums[i - 1]) continue
        twoSum(i)
    }
    return result
}
```
##### Time complexity: O(n^2)
We iterate through each value in `nums` for O(n), and call the two sum algorithm each time for O(n), giving O(n^2). Additionally, the sort (implemented in JS as merge sort (Mozilla) or quicksort (other)) is O(n^2).
##### Space complexity: O(1)
There are no growing data structures, and the sort is performed in-place.

---

# Reorder List
### Problem
You are given the head of a singly linked-list. The list can be represented as:
`L0 → L1 → … → Ln - 1 → Ln`
Reorder the list to be on the following form:
`L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …`
You may not modify the values in the list's nodes. Only nodes themselves may be changed.
### Example
```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```
### Solution
**Data structure**: none (in-place)
##### Description
We can break this down into 3 necessary steps:
1. Split the full list into first-half and second-half lists
    For this we can use two pointers as we traverse the list, one "slow" pointer and one "fast" pointer incrementing at double the rate.
2. Reverse the second-half list
3. Merge the two lists
    We point the current node of the first list to the current node of the second list, then the current node of the second list to the next node of the first list.
##### Code
```javascript
var reorderList = function(head) {
    // split into two lists, create pointers for second list
    let start = head
    let end = head
    while (end && end.next) {
        start = start.next
        end = end.next.next
    }
    // reverse second list
    let prev = null
    let curr = start
    while (curr) {
        const next = curr.next
        curr.next = prev
        prev = curr
        curr = next
    }
    // merge two lists
    let first = head
    let second = prev
    while (second.next) {
        let tmp = first.next
        first.next = second
        first = tmp
        tmp = second.next
        second.next = first
        second = tmp
    }
}
```
##### Time complexity: O(n)
We make one initial pass through the linked list (O(n)), then a pass through the latter half to reverse it (O(1/2 n)), then a pass through both halves simultaneously (O(1/2 n)) for a total of O(2n).
##### Space complexity: O(1)

---

# The kth Factor of n
### Problem
Given two positive integers n and k.

A factor of an integer n is defined as an integer i where n % i == 0.

Consider a list of all factors of n sorted in ascending order, return the kth factor in this list or return -1 if n has less than k factors.
### Example
```
Input: n = 12, k = 3
Output: 3
Explanation: Factors list is [1, 2, 3, 4, 6, 12], the 3rd factor is 3.
```
### Solution
**Data structure**: array
##### Description
We can find all factors of `n` by iterating from 1 to `sqrt(n)`. We can create one array to store all of these initial factors, and a separate array to store their complements. Then we can reverse the complements array, concatenate the two, and find our answer.
##### Code
```javascript
var kthFactor = function(n, k) {
    const factors = []
    const complements = []
    const sqrt = Math.trunc(Math.sqrt(n))
    for (let i = 1; i <= sqrt; i += 1) {
        if (n % i === 0) {
            factors.push(i)
            // skip duplicate in the case of a square
            if (n / i !== i) complements.push(n / i)
        }
    }
    
    complements.reverse()
    factors.push(...complements)
    
    return k > factors.length ? -1 : factors[k - 1]
}
```
##### Time complexity: O(sqrt(n))

##### Space complexity: O(sqrt(n))

---

# Number of Islands
### Problem
Given an `m` x `n` 2D binary grid grid which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Example
```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```
### Solution
**Data structure**: none
##### Description
Iterate through the 2D array. At each `"1"` encounter, increment the island counter and run depth-first search to mark the whole island as seen (`"0"`).
##### Code
```javascript
var numIslands = function(grid) {
    const dfs = (row, col) => {
        if (
            row < 0 ||
            col < 0 ||
            row >= grid.length ||
            col >= grid[0].length ||
            grid[row][col] === '0'
        ) return
        
        grid[row][col] = '0'
        dfs(row - 1, col)
        dfs(row + 1, col)
        dfs(row, col - 1)
        dfs(row, col + 1)
    }
    
    let islands = 0
    for (let row = 0; row < grid.length; row += 1) {
        for (let col = 0; col < grid[0].length; col += 1) {
            if (grid[row][col] === '1') {
                islands += 1
                dfs(row, col)
            }
        }
    }
    
    return islands
}
```
##### Time complexity: O(n x m)

##### Space complexity: O(1)


---
# Max Area of Island
### Problem
You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value `1` in the island.

Return the maximum area of an island in grid. If there is no island, return `0`.
### Example
```
Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
```
### Solution
**Data structure**: 
##### Description
The previous number of islands algorithm can be slightly modified to utilize two static counters for overall max size and temporary size.
##### Code
```javascript
var maxAreaOfIsland = function(grid) {
    let max_size = 0
    let max_tmp = 0
    
    const dfs = (row, col) => {
        if (
            row < 0 ||
            col < 0 ||
            row >= grid.length ||
            col >= grid[0].length ||
            grid[row][col] === 0
        ) return

        grid[row][col] = 0
        max_tmp += 1
        
        dfs(row - 1, col)
        dfs(row + 1, col)
        dfs(row, col - 1)
        dfs(row, col + 1)
    }
    for (let row = 0; row < grid.length; row += 1) {
        for (let col = 0; col < grid[0].length; col += 1) {
            if (grid[row][col] === 1) {
                dfs(row, col)
                
                if (max_tmp > max_size) max_size = max_tmp
                max_tmp = 0
            }
        }
    }

    return max_size
}
```
##### Time complexity: O(n x m)

##### Space complexity: O(1)

---

# Linked List Cycle II
### Problem
Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.
### Example
```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```
### Solution
**Data structure**: none
##### Description
We split the solution into two steps: 1) find the initial intersect between a fast and slow pointer and 2) find the connection point. For some mathematical reason this works out.

> Alternatively, use Python and create a hashmap of pointers. When a pointer is reached that is already in the map, this is the connection point.
##### Code
```javascript
var detectCycle = function(head) {
    if (!head || !head.next) return null
    let slow = head
    let fast = head
    while (fast && fast.next) {
        slow = slow.next
        fast = fast.next.next
        if (slow === fast) {
            slow = head
            while (slow !== fast) {
                slow = slow.next
                fast = fast.next
            }
            return fast
        }
    }
    return null
}
```
##### Easy Python Solution
```python
def detectCycle(self, head):
        """
        :type head: Listnode
        :rtype: Listnode
        """
        seen = set()
        while head:
            if head in seen:
                return head
            seen.add(head)
            head = head.next
        return None
```
##### Time complexity: O(n)
Not even going to bother trying to write the proof for this one tbh.
##### Space complexity: O(1)

---

# House Robber
### Problem
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.
### Example
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```
### Solution
**Data structure**: none
##### Description
We know that at each house, we can either rob and skip the next house, or skip the current house. We can utilize dynamic programming to memoize results up until certain points, or the maximum bounty until a certain index in `nums`. At `nums[i]`, the maximum bounty would be `Math.max(bounty[i - 1], bounty[i - 2] + nums[i])`, so we can loop through `nums`, creating our dynamic programming array as we go. Or, for constant spatial complexity, we can simply save the two bounty values we'll need at any given time and overwrite them as we go.
##### Code
```javascript
var rob = function(nums) {
    if (!nums.length) return 0
    if (nums.length === 1) return nums[0]
    
    let first = nums[0]
    let second = Math.max(nums[0], nums[1])
    
    for (let i = 2; i < nums.length; i += 1) {
        const tmp = Math.max(second, first + nums[i])
        first = second
        second = tmp
    }
    return second
}
```
##### Time complexity: O(n)
We will always have to iterate through the entire array.
##### Space complexity: O(1)


---

# Longest Consecutive Sequence
### Problem
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.
### Example
```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```
### Solution
**Data structure**: hashset
##### Description
We can create a set of all integers in `nums` for O(1) lookup. Then, we can iterate through `nums`. If `nums[i] - 1` does not exist in the hashset, we know this is the start of a potential sequence. We can increment this value until we no longer find it in the set, and keep a record of the total sequence value seen.
##### Code
```javascript
var longestConsecutive = function(nums) {
    if (nums.length < 2) return nums.length
    const set = new Set(nums)
    let max = 1
    for (let i = 0; i < nums.length; i += 1) {
        if (!(set.has(nums[i] - 1))) {
            let curr = 1
            let j = nums[i] + 1
            while (set.has(j)) {
                curr += 1
                j += 1
            }
            max = max > curr ? max : curr
        }
    }
    return max
}
```
##### Time complexity: O(n)

##### Space complexity: O(1)


---

# Longest Increasing Subsequence
### Problem
Given an integer array nums, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].

### Example
```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```
### Solution
**Data structure**: array (dynamic programming)
##### Description
At each index, the longest increasing subsequence can be calculated based on the numbers leading up to it. Because of this, we can use dynamic programming to store results from earlier computations. We can initialize a list of length `n` filled with `1`s, since each index at least has a subsequence containing itself. Then, we can calculate each step by looking at previous values that are lower than the current value and seeing if adding the current value to the previous subsequence gives a longer overall subsequence.
##### Code
```javascript
var lengthOfLIS = function(nums) {
    if (nums.length === 1) return 1
    
    const dp = new Array(nums.length).fill(1)
    for (let i = 1; i < nums.length; i += 1) {
        for (let j = 0; j < i; j += 1) {
            if (nums[i] > nums[j]) dp[i] = Math.max(dp[i], dp[j] + 1)
        }
    }
    
    return Math.max(...dp)
}
```
##### Time complexity: O(n^2)

##### Space complexity: O(n)


---

# Lowest Common Ancestor of a Binary Tree
### Problem
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
### Example
```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```
### Solution
**Data structure**: none
##### Description
We can traverse the tree using DFS, looking to identify both the `p` node and `q` node. At each node, we recurse on its two children. If they are found, the result of this recursive call is `true`. When we've found both, we can set our result variable to the node which contains both as children. Since the recursive calls are resolved bottom-up, the result variable will be replaced with the lowest common ancestor.
##### Code
```javascript
var lowestCommonAncestor = function(root, p, q) {
    let result = null
    const recurse = (node) => {
        if (!node) return false
        const left = recurse(node.left)
        const right = recurse(node.right)
        const mid = node.val === q.val || node.val === p.val
        if (left + right + mid >= 2) result = node
        return left || right || mid
    }
    recurse(root)
    return result
}
```
##### Time complexity: O(n)

##### Space complexity: O(n)
Call stack uses O(n) space

---

# Search in Rotated Sorted Array
### Problem
There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is rotated at an unknown pivot index `k (0 <= k < nums.length)` such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with O(log n) runtime complexity.
### Example
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```
### Solution
**Data structure**: none
##### Description
We can use a form of binary search to find the pivot index which (if it's not `0`) will be found when `nums[mid] > nums[mid + 1]`. Once we've found the pivot index, we can use a binary search on the section of `nums` likely to contain the `target` (first section if `nums[0] < target`, second section otherwise, unless `nums[pivot] === target`).
##### Code
```javascript
var search = function(nums, target) {
    if (nums.length === 1) return nums[0] === target ? 0 : -1
    const find_pivot = (left, right) => {
        if (nums[left] < nums[right]) return 0
        
        while (left <= right) {
            let mid = Math.trunc((left + right) / 2)
            if (nums[mid] > nums[mid + 1]) return mid + 1
            if (nums[mid] < nums[left]) right = mid - 1
            else left = mid + 1
        }
    }
    const pivot = find_pivot(0, nums.length - 1)
    const binary_search = (left, right) => {
        while (left <= right) {
            let mid = Math.trunc((left + right) / 2)
            if (nums[mid] === target) return mid
            if (nums[mid] > target) right = mid - 1
            else left = mid + 1
        }
        return -1
    }
    if (nums[pivot] === target) return pivot
    if (pivot === 0) return binary_search(0, nums.length - 1)
    return nums[0] <= target ? binary_search(0, pivot) : binary_search(pivot, nums.length - 1)
}
```
##### Time complexity: O(logn)
Finding the pivot requires O(logn), and finding the `target` afterwards requires O(logn) again.
##### Space complexity: O(1)
These helper functions can be written recursively, but writing them iteratively saves space on the call stack.

---

`cmd-shift-v` to preview
