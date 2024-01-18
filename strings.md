**Isomorphic Strings**: 
- check map keys for existance of `s[i]`, if present value must be same as `t[i]`, if not present check if `t[i]` present in map values, if present if must be equal to `s[i]` (easier to do in Java)
- track last occured index in 2 hashes each of a string and they should be same, if not same then not isomorphic

**Longest Common Prefix**: multiple approaches
- check pairwise

**Longest Palindromic Substring**: for every `i`, check for odd length palindromes centered at `str[i]`, then check for even length palindromes centered at `str[i]` & `str[i + 1]` (two centers for even length). Inc-dec in left-right direction and keep matching until there is a match

**Count Number of Substrings**: `n * (n + 1) / 2`, for string `abc` substrings are `{"a", "b", "c", "ab", "bc", "abc"}`
```txt
no. of substrings of length 1 + no. of substrings of length 2 + ... + no. of substrings of length n
=>  n + (n - 1) + ... + 1 (sum of first n natural numbers)
=> n * (n + 1) / 2

Generalizing - no. of substrings of length k is (n - k + 1)
```

**Operations on Substrings**: substring with a particular property like a certain sum, pattern, etc.
- 3-loops (cubic TC)
- 2-loops (quadratic TC)
- two-pointer (sliding window) (linear TC)
