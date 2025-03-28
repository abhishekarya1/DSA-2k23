**Isomorphic Strings**: 
- using one map, two scans - scan both strings and look for previous occurance of char `s[i]` and its corresponding mapping `t[i]`, if seen previously and mapping doesn't match, return false, else if not seen previously then store mapping. Do this again for string `t`.
- using one map, one scan: check map keys for existence of `s[i]`, if present (i.e. seen before) its value must be same as `t[i]`, if not present check if `t[i]` present in map values (i.e. seen before as map values contain all chars from `t` so far), if present that means it already corresponds to some other char (since `s[i]` is first occurance) and strings are not isomorphic (return false), if not present then that means both `s[i]` and `t[i]` are occuring for the first time (create a mapping)
- using two hashes of size 256: track last occured index in 2 hashes each of a string and they should be same, if not same then not isomorphic

**Longest Common Prefix**: multiple approaches
- check pairwise

**Longest Palindromic Substring**: for every `i`, check for odd length palindromes centered at `str[i]`, then check for even length palindromes centered at `str[i]` & `str[i + 1]` (two centers for even length). Inc-dec in left-right direction and keep matching until there is a match

**Count Number of Substrings**: `n * (n + 1) / 2`, for string `abc` substrings are `{"a", "b", "c", "ab", "bc", "abc"}`
```txt
no. of substrings of length 1 + no. of substrings of length 2 + ... + no. of substrings of length n
=>  n + (n - 1) + ... + 1 (sum of first n natural numbers)
=> n * (n + 1) / 2

Generalizing - no. of substrings of length k is = n - (k - 1) = n - k + 1
```

**Operations on Substrings**: substring with a particular property like a certain sum, pattern, etc.
- 3-loops (cubic TC)
- 2-loops (quadratic TC)
- two-pointer (sliding window) (linear TC)

**Roman Numeral to Decimal**: create map of known roman numerals, start from the back (`i = n-1`) and on every numeral check if (`num[i-1] < num[i]`) then subtract them (cases like `IV`, `IX`, `CD`) to get digit to add to result, if condition isn't true (normal roman numeral) just convert and add current digit

**atoi()** and **Decimal to Roman Numeral**: they are bad questions, lots of edge case handling with `if-else` (time wasters)

**Reverse Vowels in a String**: land pointers on only vowels and swap each time [link](https://leetcode.com/problems/reverse-vowels-of-a-string/submissions/1156476438/)

--- 
**Extract words from a String**: two ways, best way:
```cpp
int i = 0;
while(i < s.length()){
  while(i < s.length() && s[i] == ' ') i++;
  while(i < s.length() && s[i] != ' ') temp += s[i++];

  v.push_back(temp);

  temp = "";
  i++;
}
```
Slightly naive way, prints `""` unnecessarily on every space, so do a check on `temp` before inserting:
```cpp
int i = 0;
  while(i < s.length()){
    if(s[i] == ' '){
      // if(temp.size() > 0)
      v.push_back(temp);
      temp = "";    
    }
    else temp += s[i];
    i++;
}
```

**Number of flips to make binary string alternating 0 and 1**: use index even-odd property `0101010...` to count mismatches with the given string, mismatches with `1010101...` will be `n - count`, return minimum of those two

---

**Custom Sort String** ([link](https://leetcode.com/problems/custom-sort-string/)): duplicates in `s` cause issue here in valid ordering. Instead of looking for `s` in map built from `order` dict (first approach; more natural but fails). Create a map of `s` freqCount and build string by iterating on `order` dict and at the end append the remaining chars in `s` not present in `order` dict. Hint is that `order` can be of max length 26, while `s` can be much larger so we its better if we choose to primarily traverse on `order` dict.

### Stack and Parentheses Problems

**Make The String Great** ([link](https://leetcode.com/problems/make-the-string-great/description/)): the challenge here is that when an invalid pair is detected and removed, how do we go back and check if removal has caused another pair to form at our current pointer's left hand side
- Make multiple passes using `while(1)` and skip invalid pairs in each iteration until such pass it done in which there is no skip/fix done in that pass (use boolean `flag`)
- Use a stack and keep comparing everytime to stack top if an invalid pair is forming

**Minimum Remove to Make Valid Parentheses** ([link](https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/)): here we are trying not to remove the valid pairings but excessive parentheses. The standard trick to solve this is to scan the string twice (once form left to right and once the other way). The expected order is `(`s and then `)`s to form valid pairs, any closing char that leads to violation of count `openCnt < 0` is an excessive closing char. After the first scan we're supposed to get `openCnt = 0` if eveything is valid and there are no excess opening chars. In second scan we mark excessive opening chars if there are any i.e. till `openCnt >= 0`
- Mark excessive chars with `*` in both scans: when copying over to `ans` string, skip `*` chars (don't copy them)
- Use stack and skip pushing excessive chars into it: put stack contents in ans string and skip opening chars (this is the equivalent to scan from right to left), the reverse of stack ordering (`rev(ans)`) eventually is the final ans

**Valid Parenthesis String** ([link](https://leetcode.com/problems/valid-parenthesis-string/)): `*` char here is a wildcard char and can be taken as either `(` or `)`
- Greedy - take `cntMin` and `cntMax` denoting min and max counts of opening parentheses if all `*` chars are taken as `)` and `(` respectively. Update counters for both based on chars encountered. If `maxCnt` becomes `< 0` that means there are more closing parentheses and we can't do anything. If `minCnt < 0` that means we can re-consider some of the previous `*` that were taken as `)` as `(` (relaxation) so we set `minCnt = 0` (1 incremented). At the last we check if `minCnt == 0` because if we do relaxation too much we will end up with `minCnt < 0`. And `minCnt > 0` means there are still opening parentheses to be paired (excessive).
