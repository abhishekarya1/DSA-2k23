```txt
Subarray: contiguous part of an array, must have atleast one element. Also known as a "substring".

total no. of subarrays of all sizes = total no. of subarrays of size n + total no. of subarrays of size n-1 + ... + total no. of subarrays of size 1
  => 1 + 2 + ... + n
  => n * (n + 1) / 2

By convention, we don't consider empty subarray [] in total subarrays count.

---

Subsequence: sequence derived from the array by deleting zero or more elements, without changing the relative order of remaining elements. Also known as a "subset".
 
total no. of possible subsequences of all sizes = 2^n (power set)

By convention,, empty subsequence is counted in this.
```

- usually subarray/runlength problems
  - brute = 3 loops or 2 loops
  - better = 2 loops (some logic trimmed)
  - optimal#1 = `prefixSum[]` approach (only when subarray sum is increasing. Ex - no negatives etc.)
  - optimal#2 = simple dynamic SW with two-pointers / fixed-sized SW using Queue / some smart algorithm like Kadane

### Subarray Templates
```cpp
/* using 3 for loops */

// start point for subarrays
for (int i = 0; i < n; i++) {
    // end point for subarrays
    for (int j = i; j < n; j++) {
        // print subarrays between current start and end points
        for (int k = i; k <= j; k++){
            cout << arr[k] << " ";
        }
        cout << endl;
    }
}
```

```cpp
/* using 2 for loops - it can't print out all subarrays end-to-end but can process each subarray at thier respective endpoints i.e. j */

// start point for subarrays
for (int i = 0; i < n; i++) {
    // end point for subarrays
    for (int j = i; j < n; j++) {
        cout << "Subarray starts at : " << i << " and ends at: " << j;		// we process arr[j] here
        cout << endl;
    }
}
```

**NOTE**: range `j = [0 - i]` won't work for the second (inner) FOR loop, it has to be combinations of start points (given by `i`) and end points (given by `j`)

[Subarray 2 FOR Loop Approach Diagram - Hand Drawn](https://imgur.com/a/lT28eJV)

```cpp
/* PrefixSum approach using a map to track lastSeen sum's index */

unordered_map<int, int> lastSeen;
lastSeen[0] = -1;	// important initialization; if we sum nothing, we have sum as 0 by default

int ans = 0, sum = 0,;
for (int i = 0; i < arr.size(); i++) {

    sum += arr[i];

    if(lastSeen.find(sum) != lastSeen.end())
        ans = max(ans, i - mp[sum]);
    else
        mp[sum] = i;
}

return ans;
```

Related Problems:
- longest subarray with sum k (works even if negatives are present)
- longest subarray with 0 sum
- longest subarray with equal no. of 0s and 1s

Previous Notes:
- in [Arrays](/arrays.md) - some previously done subarray problems
- in [Strings](/strings.md) - number of subarrays derivation

---

### Sliding Window Template
0. Sliding Window Maximum - fixed window size of `k`

1. Window size is fixed `K`: maintain window size by moving 1 step on both `i` and `j`
  - https://leetcode.com/problems/sliding-window-maximum/
  - https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/
  
2. Window size is not fixed (dynamic)
  - https://takeuforward.org/data-structure/longest-subarray-with-given-sum-k/ (Solution 4; won't work if negatives are present in the array, if negatives aren't present optimal approach is SW)
  - https://leetcode.com/problems/longest-substring-without-repeating-characters/ - if char is lastSeen before, update `start = lastSeen[i] + 1`; otherwise track max and update length as `i - start + 1`
  - https://leetcode.com/problems/max-consecutive-ones-iii/
  - https://leetcode.com/problems/fruit-into-baskets/
  - https://leetcode.com/problems/longest-repeating-character-replacement/ - `chars to be replaced = windowSize - mostFreqLetterOccurance` i.e. `(r - l + 1) - mostFreqLetter` at each new element addition to the window, no need to decrease `mostFreqLetterOccurance` (and find actual max occurance in the current window) as it won't affect the final answer [ref video with timestamp](https://youtu.be/gqXU1UyA8pk?t=840)

3. Count number of subarrays/substrings with exactly K: use `func(arr, k) - func(arr, k - 1)` to get for "exactly" K, exclusively use `while` loop templates in this type of problems as we only want valid subarray boundaries for proper calc. Sliding window approach gives AT MOST answers. We end up with a valid subarray that is either less than or equal to (`<= k`) at every step and we can sum them all up as shown below.

```txt
total no. of subarrays of all sizes = n * (n + 1) / 2

but here we calc no. of subarrays on every step by summation of = (r - l + 1) at every valid window boundaries
this formula gives the no. of subarrays ending at index r, since we accept counting of < goal subarrays (as well as == goal)

the same formula was used to calc and track max subarray size in problems of above type so don't get confused
```
Handle `goal < 0` cases in this ATMOST kind of problems, `return 0` in that case.

- https://www.geeksforgeeks.org/count-number-of-substrings-with-exactly-k-distinct-characters/
- https://leetcode.com/problems/binary-subarrays-with-sum/
- https://takeuforward.org/arrays/count-subarray-sum-equals-k/ (can be solved with preSum map of counts too if no negatives are present)
- https://leetcode.com/problems/count-number-of-nice-subarrays/
- https://leetcode.com/problems/length-of-longest-subarray-with-at-most-k-frequency/ (since we check `nums[i]` here; only WHILE template works, not IF template)
- https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/: we do `n - i` here because consider `abc|bca`, valid substring is on the left of `|` and substrings possible with that (atleast) are `abc`, `abcb`, `abcbc`, `abcbca` i.e. `1 + remaining chars in string` a.k.a `n - i`.

4. Longest subarray/substring with exactly K: we can use the AT MOST template used to count subarrays above, but only track max when condition is `== k`.
- https://www.geeksforgeeks.org/find-the-longest-substring-with-k-unique-characters-in-a-given-string/

Since we're tracking maxLen, why should it even matter that we are skipping calc for `< k` cases? It mattered in subarray number calculation, but here we're supposed to find the maximum length of the subarray. There can be a case when there are never k (say `k = 3`) distinct characters in the whole array `str = [aaabbaaa]` in which case the final ans (`maxLen`) will be `len(str)` which is not correct.

5. STRICTLY LESS THAN kind of problems can also be solved using the same template as above:
- [Subarray Product Strictly Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/): modify condition as `while(j <= i && prod >= k)` to count less and equal subarrays, the first part of condition `j <= i` is added for edge case `k = 0` as we start with `prod = 1` and it will always trigger while loop and move `j` to the right of `i`

**NOTE**: `j` can cross `i` if sum/prod remains `<= k` at every step, ex `arr = [1,1,1] with prodK = 1`, so we place the condition `j <= i` to have valid `i` and `j` for calc after the loop.

6. OTHER kind of problems of SW
- https://leetcode.com/problems/count-subarrays-with-fixed-bounds: init all 3 idx with `-1`, this is solved by fixing `i` pointer on `minK` or `maxK` and calc count of valid subarrays as length between latest violating index `j` (exclusive) to least recently seen `minK` or `maxK` index, if length is valid (`>= 0`) then add length to `ans` (to count number of valid subarrays ending at current index)

**NOTE**: the code `max(0, min(minIdx, maxIdx) - j)` is small at first glance but actually goes much deeper (ofc as its a LC Hard!). The `min(minIdx, maxIdx)` ensures that we have both the `minK` and `maxK` elements in subarray till current element `i` and subtraction with `j` and subsquent length `>= 0` (or `max()`) check ensures that `minIdx`/`maxIdx` appears to the right of `j` (bad index). The actual valid subarray is from `minIdx`/`maxIdx` to current index! But count of elements between them is added because they each form a unique subarray with current valid subarray ending at current element `i`.

### Dynamic Sliding Window Templates
we need only 2 types of fruit in our SW (all templates are equivalent)

In `while` loop templates, `j` can go out of bounds in some problems where we it might not be straightforward to calculate loop variable (like in Longest Repeating Character Replacement). So `if-else` template is suited better. USE THIS WHEN ANSWER CAN BE EVENTUALLY REACHED (MAXIMUM TRACKING PROBLEMS), and needn't be immediate. 

> We never need to shrink the window size (instantly to a valid one; using while loop) because even if a valid smaller window is found, it doesn't change the max window size. So we can just move left pointer by 1 step if window is invalid (thus next window is still the same length).

For subarray problems where we need to count no. of valid subarrays for every step, `while` loop template is better. USE THIS WHEN WE NEED TO MAKE VALID ANSWER ON EVERY STEP. We can also use this always.

Template#1: calculating ans in the same step
```cpp
int totalFruit(vector<int> &fruits){

    unordered_map<int, int> mp;

    int j = 0, ans = 0;
    for (int i = 0; i < fruits.size(); i++){
        mp[fruits[i]]++;

        while (mp.size() > 2){
            mp[fruits[j]]--;
            if (mp[fruits[j]] == 0)
                mp.erase(fruits[j]);
            j++;
        }

        ans = max(ans, i - j + 1);
    }
    return ans;
}
```

Template#1A: calculating ans in the next step
```cpp
int totalFruit(vector<int> &fruits){

    unordered_map<int, int> mp;

    int j = 0, ans = 0;
    for (int i = 0; i < fruits.size(); i++){
        mp[fruits[i]]++;

        if (mp.size() <= 2)
            ans = max(ans, i - j + 1);

        while (mp.size() > 2){
            mp[fruits[j]]--;
            if (mp[fruits[j]] == 0)
                mp.erase(fruits[j]);
            j++;
        }
    }
    return ans;
}
```

Template#2: if-else stepping, no while loop. calc ans in next step (eventually gets the ans, may not be in current step)
```cpp
int totalFruit(vector<int> &fruits){

    unordered_map<int, int> mp;

    int j = 0, ans = 0;
    for (int i = 0; i < fruits.size(); i++){
        mp[fruits[i]]++;

        if (mp.size() <= 2)
            ans = max(ans, i - j + 1);

        else{
            mp[fruits[j]]--;
            if (mp[fruits[j]] == 0)
                mp.erase(fruits[j]);
            j++;
        }
    }

    return ans;
}
```

Template#3: **BEST** using IF instead of WHILE loop, calc ans in same step (eventually gets the ans, may not be in current step)

**NOTE**: this template is equivalent to WHILE loop template only when condition is not dependent on `arr[i]`, thats why both templates work for "Fruits into Baskets" problem but not for "Length of Longest Subarray With at Most K Frequency" problem (only WHILE loop template works there).

```cpp
int totalFruit(vector<int> &fruits){

    unordered_map<int, int> mp;

    int j = 0, ans = 0;
    for (int i = 0; i < fruits.size(); i++){
        mp[fruits[i]]++;

        if (mp.size() > 2){
            mp[fruits[j]]--;
            if (mp[fruits[j]] == 0)
                mp.erase(fruits[j]);
            j++;
        }

        ans = max(ans, i - j + 1);
    }
    return ans;
}
```

Template#4: for **ATLEAST** problem like [Count Subarrays Where Max Element Appears at Least K Times](https://leetcode.com/problems/count-subarrays-where-max-element-appears-at-least-k-times), differs in just window size calculation step after WHILE loop
```cpp
int count(vector<int>& nums, int e, int k) {
    int j = 0, cnt = 0, ans = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] == e)
            cnt++;

        while (cnt >= k) {
            if (nums[j] == e)
                cnt--;
            j++;
        }

        // between j and i (both inclusive) is less than k subarray (since we remmoved exactly k and greater than k subarrays using WHILE loop condition)
        // so remaining part i.e. before j (exclusive) will be atleast k (exactly k and greater than k) subarray
        ans += j;
    }
    return ans;
}
```

Alternatively, we can count `>= k` subarrays and subtract from total subarrays to get count of ATLEAST k subarrays:
```cpp
int count(vector<int>& nums, int e, int k) {
    int j = 0, cnt = 0, ans = 0;
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] == e)
            cnt++;

        while (cnt >= k) {
            if (nums[j] == e)
                cnt--;
            j++;
        }
        ans += i - j + 1;
    }

    // count of atleast k subarrays
    int totalSubArr = (nums.size() * (nums.size() + 1)) / 2;
    return totalSubArr - ans;
}
```

### Fixed Size Window Templates
**Template#1**: using a `queue<>` data structure

**Sliding Window Maximum**: find max in each fixed window size of `k`
```txt
queue<int> always stores indices of elements in queue, we pop out all the smaller element and keep only max element in the current window (maintaining monotonicity)
1. pop index older than k (single element removal)
2. pop all elements in queue less than the current array element (adding element in window; maintaining monotonicity) (we are left with either with elements greater than current or empty queue after this)
3. push current element index (i) (this is the max element index of current window)
4. push element at i (arr[i]) (but only on and after the first window is done; i >= k - 1)
```

TC = `O(n * k)`, SC = `O(n)` (for queue; mandatory)

**Template#2**:
```cpp
int maxScore(vector<int>& cardPoints, int k) {
      int n = cardPoints.size(), sum = 0, ans = 0;
      for(int i = 0; i < n; i++) {

          sum += cardPoints[i];

          // after window size is reached, it will be maintained
          if(i - j + 1 == k) {
              ans = max(ans, sum);    // track max
              sum -= cardPoints[j];
              j++;
          }
      }
      return ans;
}
```

## Two Pointers
**TP in-place**: many approaches are discussed in Array notes using swaps

**TP with Linear Space**: a `for` loop is used to populate `ans` array, with `low` and `high` pointers on the input array
- [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/) - two-pointer approach works for sorting here because the input array `nums` is sorted (in increasing order) and the first and last elements' squares will be biggest elements in the ans array (if negatives are present), smaller squares are towards the middle. Start populating `ans` array from the end.
- [Bag of Tokens](https://leetcode.com/problems/bag-of-tokens/) - greedy (sorting) and two start and end pointers (has own sol post)
- [Minimum Length of String After Deleting Similar Ends](https://leetcode.com/problems/minimum-length-of-string-after-deleting-similar-ends/) - on same `i` and `j` char, remove same chars repeatedly from each end. Do this `while(i < j)` and then calc length as `j - i + 1`. Cases to test: `aabca` (normal case), `a` (single char), `aaaa` (i crosses j in execution)
- [Container With Most Water](https://leetcode.com/problems/container-with-most-water): sort of like Trapping Rainwater but such complicated two-pointers isn't needed here. Simple two pointers will work because we're focusing on only two bars at a time and not trapping rainwater falling from above in the whole histogram. Water between two pointer, track max of `ans = max(ans, height[low] * (high - low))` if bar at `low` is smaller, or `ans = max(ans, height[high] * (high - low))` if the bar at `high` is smaller in height. Notice that the array isn't sorted but we try to maximize the first component `height[]` or the equation since we're reducing `(high - low)` everytime we move ahead/back on any one pointer, we are limited by shorter bar and not the longer one so we move pointer on the shorter one.
