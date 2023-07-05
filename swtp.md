- usually subarray/runlength problems
  - brute = 2 loops
  - better = 2 loops (some logic trimmed)
  - optimal = 1 loop

1. Window size is fixed `K`: maintain window size by moving 1 step on both `i` and `j`
  - https://leetcode.com/problems/sliding-window-maximum/
  - https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/
  
2. Window size is not fixed (dynamic)
  - https://takeuforward.org/data-structure/longest-subarray-with-given-sum-k/ (Solution 4)
  - https://leetcode.com/problems/longest-substring-without-repeating-characters/
  - https://leetcode.com/problems/max-consecutive-ones-iii/
  - https://leetcode.com/problems/fruit-into-baskets/
  - https://leetcode.com/problems/longest-repeating-character-replacement/

3. Count number of subarrays/substrings with exactly K: use `func(arr, k) - func(arr, k - 1)` to get for "exactly" K, exclusively use `while` loop templates in this type of problems. Sliding window approach gives AT MOST answers.
- https://www.geeksforgeeks.org/count-number-of-substrings-with-exactly-k-distinct-characters/
- https://leetcode.com/problems/binary-subarrays-with-sum/
- https://takeuforward.org/arrays/count-subarray-sum-equals-k/ (can be solved with preSum map of counts too)
- https://leetcode.com/problems/count-number-of-nice-subarrays/
- https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/ (AT LEAST approach): we do `n - i` here because consider `abc|bca`, valid substring is on the left of `|` and substrings possible with that (atleast) are `abc`, `abcb`, `abcbc`, `abcbca` i.e. `1 + remaining chars in string` a.k.a `n - i`.

### Dynamic Sliding Window Templates 
we need only 2 types of fruit in our SW (all templates are equivalent)

In `while` loop templates, `j` can go out of bounds in some problems where we it might not be straightforward to calculate loop variable (like in Longest Repeating Character Replacement). So `if-else` template is suited better. USE THIS WHEN ANSWER CAN BE EVENTUALLY REACHED (MAXIMUM TRACKING PROBLEMS), and needn't be immediate. 

> We never need to shrink the window size because even if a valid smaller window is found, it doesn't change the max window size. So we can just move left pointer by 1 step if window is invalid (thus next window is still the same length).

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

Template#3: **BEST** using if instead of while loop, calc ans in same step (eventually gets the ans, may not be in current step)
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
