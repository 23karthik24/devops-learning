\# Python Basics: Day 7



\## What I learned

\- `print` shows text on the screen.

\- A variable is a named box that stores a value so I can use it later. In my scripts, `name = "LUSU"` stores the text LUSU, and `hours\_per\_day = 2` stores the number 2.

\- Types: `str` is text and `int` is a whole number. `5 + 5` gives `10` because numbers are added, but `"5" + "5"` gives `"55"` because text is joined.

\- `input()` always gives back text, so I use `int()` to turn the answer into a number before doing maths.

\- `str()` turns a number into text so I can join it to a sentence with `+`.

\- I prefer joining text with `+` and `str()`. I know f-strings and `.format()` do the same job, and I can read them when I see them.



\## Errors I saw

\- \*\*SyntaxError in hello.py:\*\* I had a stray `=` after the comma in the print line. Python's error pointed at line 2 with a `^` under the `=`, so I deleted it and the script ran.

\- \*\*TypeError when I removed `str()`:\*\* Python said it can only concatenate str (not "int") to str. It means I can't join text and a number with `+`, which is why `str()` is needed.

\- \*\*7777777 when I removed `int()`:\*\* `input()` gave back the text `"7"`, so `"7" \* 7` repeated the text seven times instead of multiplying to 49.

\- \*\*An empty greeting.py ran with no output:\*\* I hadn't saved the file with Ctrl+S, so it was empty. Python doesn't complain about an empty file, it just does nothing. `type python\\greeting.py` showed me the file was empty.



\## Scripts

\- `python/hello.py`: prints a greeting

\- `python/study\_hours.py`: variables, maths and types

\- `python/greeting.py`: asks questions with `input()`



\## What confused me

\- The `{}` and `!` in `.format()` and f-strings. I worked out that `{}` is a blank that gets filled in, and the `!` is just an ordinary exclamation mark.

\- Why a script can run with no error and still print nothing (the empty file).

\- When to use `+` and `str()` compared with f-strings. I chose `+` and `str()`, and I know the others exist.

