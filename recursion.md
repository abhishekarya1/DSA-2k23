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

---

**2-D**: the main string to be traversed is traevsed by recursion calls, other by FOR loop. Ex - in phone keypad problem, we traverse digits by recursion and run FOR loop for "abc" "def" etc... Another example - combination sum problems are also this way only, there we place 1 element of the array using recursion and using FOR loop we traverse the rest of the array to find pairs for it

### Advanced Problems
**Palindrome Partitioning**: check all partition positions, if a partition's left substring (`place` to `i`) is a palindrome then recur for remaining string, else move ahead `i++` with the same position `place` as partition split position

**Word Search**: recur on string `word` as soon as the first occurance of the `word[0]` is found, traverse all four directions matching subsequent characters of `word` and if in any direction we reach a dead end (either grid boundaries, non-matching character, or an already visited cell). Edge case - mark visited in grid cell as `!` because we can traverse the grid and loop back to the same element again and use it again which is invalid. Make sure to backtrack i.e. unvisit after recursive calls.
