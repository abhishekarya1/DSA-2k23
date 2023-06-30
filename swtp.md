- usually subarray problems
  - brute = 2 loops
  - better = 2 loops
  - optimal = 1 loop with usually additional space complexity for set or map 

1. Window size is fixed `K`
  - https://leetcode.com/problems/sliding-window-maximum/
  
2. Window size is not fixed (dynamic)
  - https://takeuforward.org/data-structure/longest-subarray-with-given-sum-k/ (Solution 4)
  - https://leetcode.com/problems/longest-substring-without-repeating-characters/
  - https://leetcode.com/problems/max-consecutive-ones-iii/
  - https://leetcode.com/problems/fruit-into-baskets/


**Dynamic Sliding Window Templates**: we need only 2 types of fruit in our SW (all templates are equivalent)

Template#1:
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

Template#2:
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

Template#3:
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
