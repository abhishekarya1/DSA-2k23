## Resources
- https://hashdefine.netlify.app/dsa/bit/
- Bit Twiddling Hacks - https://graphics.stanford.edu/~seander/bithacks.html
- XOR - https://www.chiark.greenend.org.uk/~sgtatham/quasiblog/xor/ and its HN Thread (has useful tricks) - https://news.ycombinator.com/item?id=43087944

## Tips

XOR can't always be used to detect duplicate pairs, as `[1, 2, 4, 7]` has a XOR value of `0`, but there are no duplicates present! So its not a universal solution to detect if multiple single elements exist among pair duplicates. [Problem](https://leetcode.com/problems/divide-array-into-equal-pairs). It works best when there is exactly one single element among pair duplicates.
