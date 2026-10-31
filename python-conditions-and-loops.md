# Python Conditions and Loops: Days 8 and 9



## Conditions

- A comparison gives `True` or `False`. `=` stores a value, and `==` asks a question.

- `if`, `elif` and `else` let a script choose. Python goes from the top and runs only the first block whose question is true, so the strictest question goes first.

- `and` needs both sides true, `or` needs at least one, and `not` flips the answer.

- Every `if`, `elif` and `else` line ends with a colon, and the lines inside are pushed in by 4 spaces.



## The for loop

- `for day in range(1, 8):` repeats the indented lines. `range(1, 8)` gives 1 to 7: the start is included and the stop is never included. `range(3)` gives 0, 1, 2.

- The loop variable (`day`) holds a new number each round.

- A running total needs `total = 0` above the loop and `total = total + 2` inside it.



## Mistakes I made on purpose

- Missing colon after `else` gave `SyntaxError: expected ':'`.

- With `hours = 5`, `>` printed "Keep going" but `>=` printed the success message, because 5 is not bigger than 5.

- With `total = 0` inside the loop, the total printed 2 every round, because the box was emptied every round.

- With the last `print` pushed inside the loop, "Final total" printed every round instead of once.

- A misspelled file name (`pratice.py`) gave `[Errno 2] No such file or directory`.



## Scripts

- `python/conditions.py`: picks a message from the hours studied

- `python/piggy_bank.py`: adds up a daily amount over 7 days



## What confused me

- Saying the rules out loud in my own words. I can trace them on paper but find explaining them harder.

