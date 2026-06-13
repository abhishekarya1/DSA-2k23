## Quant Puzzles
https://www.quantapus.com/roadmap

### Monty Hall Problem
- Numberphile: https://youtu.be/4Lb-6rxZxx0
- MIT OCW: https://youtu.be/UgKrQ2ywVfs

Assumption is that Monty knows which door has the car and threfore deliberately avoids opening it.

Alternative single line explanations:
- Not using new information after the reveal will mean sticking to the chosen door that had 1/3 probability of having a car. Better to switch as there is now 2/3 probability of car being in the other remaning door (i.e. "probability concentration" concept in the Numberphile video). Better visualized when we imagine 100 doors.
- Switching strategy wins if and only if you originally picked a goat, and the probability of that happening is higher i.e. 2/3. So probability of not switching and winning is 1/3 whereas switching and winning is 2/3, twice as much.
- One can also work out all 6 possible scenarios and show that switching leads to win in 2/3 of them and not switching leads to win in only 1/3.


### Duck or Hell
You're getting negation of the correct answer anyways. Either you're getting unaltered answer from the other guard (if current guard is truthy) or current guard modified the answer (he's the liar). SO always pick the other door than the one they're pointing to (or saying "Yes" to the question for).
