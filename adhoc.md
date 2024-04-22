## Ad-Hoc / Implementation / Simulation
[Tic-tac-Toe](https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/solutions/441422/java-python-c-0ms-short-and-simple-all-8-ways-to-win-in-one-array/) in two arrays (one for each player) without saving board state

[Robot Return to Origin](https://leetcode.com/problems/robot-return-to-origin/) - direction change is not from POV of the robot, and moves aren't repeating after one scan, so we can just calcuate vertical and horizontal displacements and they both should be zero

[Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle/) - robot's POV and moves are repeated infinitely after one scan too. If the robot ends up facing north or displacement becomes `0` (trivial case) then only its a circle, other wise it will always proceed to go out of the grid (non-zero displacement), use clockwise direction vector `N E S W` and mod `%` operations to steer left (`(i + 3) % 4`) or right (`(i + 1) % 4`)

[Path Crossing](https://leetcode.com/problems/path-crossing/) - store `{0, 0}` origin in set and subsequently simulate and store all previously visited coordinates in a `set<pair<int, int>>`, check on each step if we've been there previously then return `true` else at the end of simulation return `false`.
