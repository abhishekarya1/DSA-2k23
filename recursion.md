**Reverse a Stack using Recursion** (not using any aux stack, only internal function call stack):
- store all elements of stack in call stack until stack is empty
- insert elements one by one in the stack BUT always insert at the bottom

```cpp
// Template#1 - reverse stack using recursion
void insertAtBottom(stack<int>& st, int x){
    if(st.empty()) {
        st.push(x);
    }

    else {
        int e = st.top();
        st.pop();
        insertAtBottom(st, x);
        st.push(e);     // replace existing stack elements (backtracking)
    }
}

void reverseStack(stack<int>& st){
    if(st.empty()) return;

    int e = st.top();
    st.pop();

    reverseStack(st);

    // access elements in reverse fashion and insert at bottom
    insertAtBottom(st, e);
}
```
**Sort a Stack using Recursion** (not using any aux stack, only internal function call stack): (just like above reverse stack approach it uses smart positional insert)
- store all elements of stack in call stack until stack is empty
- insert elements one by one in the stack BUT always insert in its sorted place (like insertion sort)

```cpp
// Template#2 - sort stack using recursion (ascending order)
void insertSorted(stack<int>& st, int x){
    if(st.empty() || x < st.top()) {
        st.push(x);
    }

    else {
        int e = st.top();
        st.pop();
        insertSorted(st, x);
        st.push(e);     // replace existing stack elements (backtracking)
    }
}

void sortStack(stack<int>& st){
    if(st.empty()) return;

    int e = st.top();
    st.pop();

    sortStack(st);

    // access elements in reverse fashion and insert at correct sorted position
    insertSorted(st, e);
}
```

---

### Subsequences
Old Notes - refer Samsung Notes PDF

### Subsequences Patterns
A subsequence is a generalization of substring in which it may not be contiguous but order should be maintained
- generate all subsequences of size n (`vector<int>`)
- generate all subsequence with sum k (`vector<int>`)
- check if a subsequence exists with sum k (`bool`)
- count all subsequences with sum k (`int`)

Two templates:
1. Pick/NotPick - subsequences (all tree nodes have 2 children except leaves)
2. FOR loop - subsequences (tree nodes have atmost `str.size()` children), permutations & combinations, there is no base case as such, not-pick step is absent (this is used when we want to skip duplicates)

Base cases: In Pick/NotPick it is `i == arr.size()`. In FOR loop approach base case isn't required since FOR loop range takes care of array traversal bounds
```cpp
void genSubseq(int i, string curr, string str){
    if(i == str.length()) {            // base case
        cout << curr << "\n";
        return;
    }

    genSubseq(i + 1, curr + str[i], str);    // pick
    genSubseq(i + 1, curr, str);            // not pick
}

void genSubseqForLoopVersion(int i, string curr, string str){
    cout << curr << "\n";        // no base case
    
    for(int j = i; j < str.length(); j++){        // for loop; notice start index
        genSubseq(j + 1, curr + str[j], str);     // only pick, no not pick step
    }
}

// function calls - in main()
genSubseq(0, "", str);
genSubseqForLoopVersion(0, "", str);
```

TC = `O(2^n)`

[Link to Recursion Tree Diagrams](https://imgur.com/a/02PcLRh)

Whenever we require to remove duplicates (uniqueness), we `sort` and then use the FOR loop approach. Ex - ComboSum2, SubsetSum2. 

Skip condition: `if(i > p && arr[i] == arr[i - 1]) continue;`. We can further optimize the FOR loop using `if(arr[i] > target) break;` (if current element can't be part of the answer then remaining array can't be too; since array is sorted).

### Other Patterns
**Generate Combinations** - they are nothing but subsequences of fixed length `len` i.e. in the recursion tree of subsequences don't go all the way to the leaves but `return` when length of curr hits `len` (combination length)
```cpp
void genCombinations(int i, string curr, string str, int len) {
    if(curr.size() == len) {        // only thing diff from above subseq template
        cout << curr << "\n";
        return;
    }
    
    for(int k = i; k < str.length(); k++){
        genCombinations(k + 1, curr + str[k], str, len);
    }
}

// call in main
genCombinations(0, "", "abcde", 4);
```

**Generate Permutations** - all permutations have equal length, swap characters each time and we don't even need a `curr` to store permutations. TC = `O(n!)`
```cpp
void genPermutations(int i, string str) {
    if(i == str.size()) {
        cout << str << "\n";
        return;
    }
    
    for(int k = i; k < str.length(); k++){
        swap(str[i], str[k]);            // swap
        genPermutations(i + 1, str);     // notice first param here
        swap(str[i], str[k]);            // backtrack
    }
}

// call in main
genPermutations(0, "abc");
```

[Link to Recursion Tree Diagram](https://imgur.com/a/6BUdglp)

[Sequential Digits](https://leetcode.com/problems/sequential-digits/) - `1234`, `2345`, `3456` and so on.
```cpp
void helper(int i, int n, int low, int high, vector<int> &ans){
    if(i >= 10 || n > high) return;

    n = n * 10 + i;

    if(n >= low && n <= high) ans.push_back(n);

    helper(i + 1, n, low, high, ans);
}

// call in main function
vector<int> ans;
int n = 0;
for(int i = 1; i <= 9; i++)
    helper(i, n, low, high, ans);

sort(ans.begin(), ans.end());
return ans;
```

**Letter Combinations of a Phone Number**: normal combination template actually does combination on own `str = "abc"` using FOR loop as `"a"` and `"bc"` producing `"ab"` and `"bc"`. Same way we do for `str = "23"`, but expand str as `"abcdef"` using a dictionary of key-letter maps and produce all `len(str)` sized combinations using the same template [problem](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

(Old Explanation) **2-D**: the main string to be traversed is traversed by recursion calls, other by FOR loop. Ex - in phone keypad problem, we traverse digits by recursion and run FOR loop for "abc" "def" etc... Another example - combination sum problems are also this way only, there we place 1 element of the array using recursion and using FOR loop we traverse the rest of the array to find pairs for it

### Advanced Problems
**Palindrome Partitioning**: check all partition positions, if a partition's left substring (`start` to `i`) is a palindrome, put it in ans vector and then recur for remaining string moving ahead `i + 1` in recursion call

**Word Search**: recur on string `word` as soon as the first occurance of the `word[0]` is found, traverse all four directions matching subsequent characters of `word` and if in any direction we reach a dead end (either grid boundaries, non-matching character, or an already visited cell). Edge case - mark visited in grid cell as `!` because we can traverse the grid and loop back to the same element again and use it again which is invalid. Make sure to backtrack i.e. unvisit after recursive calls. This is nothing but DFS - go and check.
-  TC = `4 ^ (m*n)` since at each cell we move in 4 directions

**Rat in a Maze**: Nothing but simple DFS - check and go, make sure to backtrack if further movement is not possible. TC = `4 ^ (m*n)`

**N-Queens**: input is `nCol = nRow = nQueens`,  recur on `cols` and FOR loop on `rows`, need to check only three sides (left in the same row, left upper diag, left lower diag) as we're moving from left to right. 
- TC = `O(N! * N)`. For the first queen, we have `N` choices of squares, for the second queen we need to check `N-1` choices (since one square is taken by the first queen), for the third queen we need to check for `N-2` choices, and so on till only `1` choice and we explore all choices so we take product (i.e. factorial). In practice, TC is better since we know if we cannot place a queen and we backtrack earlier than reaching `1`

```txt
Template for below problems - returns bool, make a current choice and recur on next items (rest of the data). If true is returned, that means operations were successful till the end and return true from current too, otherwise upon false do backtracking

for(i : all choices)
    if(isValid(i, data) == true)
        add data_i to ans
        if(recurFunc(data) == true)
            return true
        else
            remove data_i from ans - backtrack
```

**Word Break**: use `substr(0, i+1)` to get left word and do linear search for each left word till current index `i`, once a word is found in dict, recur with right substring (remaining string) as the og string. If right recur call returns `true` (meaning breaking successful till the end; propagated backwards) return `true` from current too, otherwise backtrack if `false` is returned
- backtracking helps covers cases like `str = "leetacode", dict = {"leet", "leeta", "code"}`, firstly "leet" will be found and we will recur for "acode" returning false after checking, then we come back and backtrack popping "leet" from ans and pushing "leeta" and recur for "code" which gives correct ans

**M-Coloring Problem**: try all colors for all nodes checking validity and recur for next node, if any of the next nodes can't be colored - backtrack on current, decolor and recolor (FOR loop's next iteration)

**Sudoku Solver**: find an empty cell and try all 9 numbers in it if valid, recur on board. If none of the numbers were placed return false, if all board traversal is done and we didn't return yet, return true

