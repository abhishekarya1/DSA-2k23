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
  - Note that in random array problem, we skip elements not equal to target since probability should be equal among all target value elements (so reservior sampling works here too if we only keep count of the target value elements)

- Shuffle the Array (such that all permutations are equally likely): [video link]( https://youtu.be/hSZARPLUSDM)
  - swap every element `arr[i]` with `arr[rand(i, n-1)]`, `i` is inclusive
  - intuition: at every element prob of that element landing up at that position should be `1/n` which isn't possible if we don't reduce the rand's range

## Geometry
### Triangle Inequality Theorem
```txt
Theorem: Sum of any two sides should be strictly greater than the third side inorder to form a triangle with them.
x + y > z
y + z > x
x + z > y
```
[Largest Perimeter Triangle](https://leetcode.com/problems/largest-perimeter-triangle/) - we don't need to find all triplets! this can be solved with a greedy approach, sort all sides, observation is that from the above three conditions, if `x y z` are sorted, we only need to check `x + y > z` and the other two conditions will be satisfied dude to sort property, track max on every valid condition

### Distance Geometry
- Euclidean Distance (diag is at `sqrt(2)` dist)
- Manhattan (Taxicab) Distance (diag is at `2` dist)
- Chebyshev Distance (diag is at `1` dist)

![grid_diagram_showing_above_three_dist](https://upload.wikimedia.org/wikipedia/commons/thumb/e/eb/Minkowski_distance_examples.svg/240px-Minkowski_distance_examples.svg.png)
![](https://iq.opengenus.org/content/images/2018/12/distance.jpg)

Usage in Chess: Rooks and Bishops use MD, Kings and Queens use CD

[Minimum Time Visiting All Points](https://leetcode.com/problems/minimum-time-visiting-all-points/) - answer is sum of chebyshev's distance (since diagonal dist is `1`) between each pair of points
