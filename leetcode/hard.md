# Trapping Rain Water
### Problem
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
### Example
```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```
### Solution
**Data structure**: none
**Technique**: two pointers
##### Description
We can maintain two pointers on either end of the array, as well as a running max on the left and right sides, since we know water can only be collected at an index if `min(leftMax, rightMax) > height[i]`. At each iteration, we increment/decrement the left/right pointer respectively, if its height value is lower than that of the other pointer. Then, we can compute the amount of water collected at that index and add it to our running total.
##### Code
```javascript
var trap = function(height) {
    if (height.length < 3) return 0
    let left = 0
    let right = height.length - 1
    let collected = 0
    let leftMax = height[0]
    let rightMax = height[height.length - 1]
    
    while (left !== right) {
        if (height[left] > height[right]) {
            right -= 1
            const tmp = Math.min(leftMax, rightMax) - height[right]
            if (tmp > 0) collected += tmp
            rightMax = Math.max(rightMax, height[right])
        } else {
            left += 1
            const tmp = Math.min(leftMax, rightMax) - height[left]
            if (tmp > 0) collected += tmp
            leftMax = Math.max(leftMax, height[left])
        }
    }
    
    return collected
}
```
##### Time complexity: O(n)
This method only requires one pass through the array.
##### Space complexity: O(1)


---
