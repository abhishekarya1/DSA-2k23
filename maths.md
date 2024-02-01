- Leap Year:
  - div by `4`, if a `100` year, it should be div by `400`
  - Ex: `1900` isn't leap, despite being div by `4`
- Prime Number: https://hashdefine.netlify.app/maths
- Sieve of Eratosthenes: Loop `i -> 2 to sqrt(n), i++`, loop `j -> i*i to n, j+=i` and untick elements
  - Time - `O(n log logn)`
- Divisors: https://hashdefine.netlify.app/maths
- Binary Exponentiation: https://cp-algorithms.com/algebra/binary-exp.html
  - derived from bitwise observation (iterative; fastest)
  - halving then squaring
  - squaring then halving (tail recursive)
  - for floating-point numbers and negative powers  
- Euclidean Algorithm: https://cp-algorithms.com/algebra/euclid-algorithm.html
  - for floating-point numbers
- Fibonacci Numbers:
  - Recursion
  - Memoisation using array
  - Iterative that keeps track of only last two elements (space optimized)
  - Binet's Formula

---

[Bulb Switcher](https://leetcode.com/problems/bulb-switcher/) - no. of odd/even divisors

[Power of Three](https://leetcode.com/problems/power-of-three/) - prime factorization keep divising by 3 until you get `1` or number not div by 3. Edge case is `n = 0` where TLE happens, make `n == 0` also `false`

--- 
### Randomized Algorithms - Reservior Sampling
- [Random LL Node](https://leetcode.com/problems/linked-list-random-node/solutions/4650025/o-n-time-o-1-space-using-reservior-sampling-randomized-algorithm-c/) and [Random Array Index](https://leetcode.com/problems/random-pick-index/) - [video link](https://youtu.be/DWZqBN9efGg)

- Shuffle the Array (such that all permutations are equally likely): [video link]( https://youtu.be/hSZARPLUSDM)
  - swap every element `arr[i]` with `arr[rand(i, n-1)]`, `i` is inclusive
  - intuition: at every element prob of that element landing up at that position should be `1/n` which isn't possible if we don't reduce the rand's range
