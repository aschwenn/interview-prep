# Sliding Window
**Data structure(s)**: array, string

Sliding window can be used to convert nested for loops into single for loops to reduce time complexity.

A window is a range of elements `[i,j)` that slides its boundaries in a certain direction.

---

# Dynamic Programming
**Data structure(s)**: array, string, ...

Dynamic programming can optimize recursive problems by storing the results of subproblems for later use. This can reduce time complexity from exponential-time to polynomial-time.

##### Steps to solving with dynamic programming

1. Create a recursive solution
2. Store any repeated computations (memoize)
3. Create bottom-up approach instead of recursive solution

##### Example

Recursive solution: O(2^n)
```javascript
const fib = (n) => {
  let result
  if (n === 1 || n === 2) result = 1
  else result = fib(n - 1) + fib(n - 2)
  return result
}
```

Memoized solution: O(n)
```javascript
const memo = {}

const fib = (n) => {
  if (n in memo) return memo[n]
  let result
  if (n === 1 || n === 2) result = 1
  else result = fib(n - 1) + fib(n - 2)
  memo[n] = result
  return result
}
```

Bottom-up approach: O(n)
```javascript
const fib (n) => {
  if (n === 1 || n === 2) return 1
  const dp = new Array(n + 1)
  dp[1] = 1
  dp[2] = 1
  for (let i = 3; i < n; i += 1) {
    dp[n] = dp[n - 1] + dp[n - 2]
  }
  return dp[n]
}
```

